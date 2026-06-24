---
title: webdav-einrichten
description: 
published: true
date: 2026-06-24T17:35:05.255Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:37:31.939Z
---

# WebDAV einrichten

# IIS

## Voraussetzungen

- Windows Server mit installierter IIS-Rolle
- Mitglied in einer Active Directory Domäne
- Administratorrechte auf dem Server
- Service-Account (Domänenkonto), z. B. `DOMAIN\svc_webdav`

## 1. WebDAV-Rolle installieren

1. Öffne den **Server Manager**
2. Gehe zu **Verwalten → Rollen und Features hinzufügen**
3. Wähle:
   - **Webserver (IIS)**
   ![webdav-001.png](/media/webdav-001.png)
   - Unter **Webserver → Gemeinsam genutzte Dateien**:
     - WebDAV Publishing
     ![webdav-002.png](/media/webdav-002.png)
   - Unter **Webserver → Sicherheit**:
   	 - Sttandardauthentifizierung
		 - Windows-Authentifizierung
     - URL-Autorisierung
4. Installation abschließen


## 2. Verzeichnis für WebDAV erstellen

`mkdir C:\WebDAV`


## 3. NTFS-Berechtigungen setzen

1. Rechtsklick auf `C:\WebDAV` → **Eigenschaften**
2. Reiter **Sicherheit**
3. Service-Account hinzufügen: `DOMAIN\svc_webdav`
4. Berechtigungen:
   - Lesen
   - Schreiben
   - Ändern

## 4. IIS Website erstellen

1. Öffne **IIS-Manager**
2. Rechtsklick auf **Sites → Website hinzufügen**
3. Einstellungen:
   - **Name**: WebDAV
   - **Physischer Pfad**: `C:\WebDAV`
   - **Port**: z. B. `8080`
   ![webdav-003.png](/media/webdav-003.png)
4. Bestätigen

## 5. WebDAV aktivieren

1. Website auswählen
2. Öffne **WebDAV Authoring Rules**
3. Klicke auf **Enable WebDAV**

## 6. Authoring-Regeln konfigurieren

1. In **WebDAV Authoring Rules**
2. Klicke **Add Authoring Rule**
![webdav-004.png](/media/webdav-004.png)
3. Einstellungen:
   - **All content**
   - **Specified users**: `DOMAIN\svc_webdav`
   - **Permissions**:
     - Read
     - Write
     - Source
![webdav-005.png](/media/webdav-005.png)
4. Klicke **WebDAV aktivieren**

## 7. Authentication konfigurieren

1. Öffne den **IIS-Manager**
2. Wähle deine Website aus
3. Öffne **Authentication**
4. Setze:
   - Anonymous Authentication → Disable
   - Basic Authentication → Enable (nur mit HTTPS empfohlen)
   - Windows Authentication → Enable (für Domänenumgebung empfohlen)
![webdav-007.png](/media/webdav-007.png)

> Wenn **Basic Authentication** und **Windows Authentication** im IIS nicht angezeigt werden, müssen diese Rollen/Features zuerst installiert werden.
{.is-info}


### Variante 1: Über den Server-Manager

1. Öffne den **Server Manager**
2. Klicke auf **Verwalten → Rollen und Features hinzufügen**
3. Gehe zu **Webserver (IIS) → Webserver → Sicherheit**
4. Aktiviere:
   - Basic Authentication
   - Windows Authentication
![webdav-006.png](/media/webdav-006.png)
5. Installation abschließen


### Variante 2: Per PowerShell

`Install-WindowsFeature Web-Basic-Auth, Web-Windows-Auth`

## 8. Authorization Rules überprüfen

1. Öffne **Authorization Rules**
2. Entferne ggf. vorhandene Regeln
3. Füge neue Regel hinzu:
   - **Allow**
   - User: `DOMAIN\svc_webdav`

> Falls „Authorization Rules“ im IIS fehlt, muss das Feature nachinstalliert werden.
> {.is-info}
{.is-info}



### Fehlendes Feature: URL Authorization

Dieses Feature stellt die „Authorization Rules“ im IIS überhaupt bereit.


#### Installation über Server-Manager

1. Öffne **Server-Manager**
2. Klicke auf **Verwalten → Rollen und Features hinzufügen**
3. Navigiere zu:
   - **Webserver (IIS)**
   - → **Webserver**
   - → **Sicherheit**
4. Aktiviere:
   - **URL Authorization**
   ![webdav-008.png](/media/webdav-008.png)
5. Installation abschließen

#### Installation per PowerShell

`Install-WindowsFeature Web-Url-Auth`

### Danach im IIS prüfen

1. IIS-Manager neu öffnen (wichtig!)
2. Deine Website auswählen
3. In der **Funktionsansicht** sollte jetzt sichtbar sein:
   - **Authorization Rules** (Autorisierungsregeln)
4. Punkt 8 wiederholen

## 9. Request Filtering anpassen (optional)

- Öffne **Request Filtering**
- Stelle sicher:
  - Keine verbotenen Extensions oder Methoden blockieren WebDAV
- Optional:
`<requestFiltering allowDoubleEscaping="true" />`

---

## 10. WebDAV testen

### Variante 1: Windows Explorer

1. Öffne Explorer
2. Rechtsklick → **Netzlaufwerk verbinden**
3. Adresse:
```
http://server:8080/
```
4. Anmelden mit:
```
DOMAIN\svc_webdav
```

---

### Variante 2: CMD

`net use Z: http://server:8080 /user:DOMAIN\svc_webdav`

---

## 11. SSL (empfohlen für Produktivbetrieb)

1. Zertifikat installieren
2. IIS → **Bindings**
3. HTTPS hinzufügen (Port 443)
4. Basic Auth nur mit HTTPS verwenden!

---

## 12. Typische Fehler & Lösungen

| Problem | Ursache | Lösung |
|--------|--------|--------|
| 401 Unauthorized | Auth falsch | Authentication prüfen |
| 405 Method Not Allowed | WebDAV nicht aktiv | Regeln prüfen |
| Zugriff verweigert | NTFS Rechte fehlen | Berechtigungen korrigieren |
| Anmeldung funktioniert nicht | NTLM/Kerberos Problem | SPN / Windows Auth prüfen |

## Ergebnis

Nach der Konfiguration kann sich der Serviceaccount anmelden und:

- Dateien lesen  
- Dateien speichern  
- Dateien bearbeiten  

---

## Optional: Best Practices

- Nur HTTPS verwenden
- Logging im IIS aktivieren
- Zugriff auf bestimmte IPs einschränken
- Serviceaccount mit minimalen Rechten betreiben

## Verbinden mit NetUse
Beim Verbinden mit den Windows Netzlaufwerk Tools ist zu beachten wie Adresse auszusehen hat.
[win7-webdav-verbinden#win7-webdav-verbinden](/de/Wiki-Seiten/Microsoft/Server/Rollen/WebDAV/win7-webdav-verbinden#win7-webdav-verbinden)

# IIS6 (alte Anleitung nur für Historie)

1. Entweder unter der Standardwebsite arbeiten oder eine eigene Website anlegen. Bei einer eigenen Website sollte noch ein Ordner im Explorer angelegt werden. In dieser Anleitung wird es nur als Standardwebsite bezeichnet.
2. Eigenschaften der Standardwebsite öffnen.
3. SSL Port angeben (Standard 443)
4. Reiter Verzeichnissicherheit auswählen
5. Unter Authentifizierung und Zugriffsteuerung überprüfen ob der Anonyme Zugriff gewährt ist.
6. Unter Sichere Kommunikation auf Serverzertifikat klicken und ein Zertifikat ausstellen für den Server wenn noch keins vorhanden ist. Die Anforderung bei einer Zertifizierungsstelle einreichen und danach das ausgestellte Zertifikat hier importieren (Vorgang abschließen)
7. Unter der Standardwebsite ein Virtuelles Verzeichnis anlegen
8. Berechtigungen des Virtuellen Verzeichnisses öffnen
9. Internetgastkonto nur Leserechte
10. Andere Benutzer gewollt Rechte verteilen und bestätigen.
11. Eigenschaften des Virtuellen Verzeichnisses öffnen
12. Unter dem Reiter Virtuelles Verzeichnis die Rechte auf Lesen / Schreiben / Verzeichnis durchsuchen / Besuche protokollieren setzen
13. Reiter Verzeichnissicherheit auswählen
14. Bei Authentifizierung und Zugriffsteuerung auf Bearbeiten klicken
15. Anonymen Zugriff deaktivieren und nur Standardauthentifizierung aktivieren.

**WebDAV TMG Regel:**
Siehe hier: [tmg-firewallregeln#webdav-mit-ssl-in-dmz](/de/Wiki-Seiten/Microsoft/Server/Rollen/TMG-UAG/tmg-firewallregeln#webdav-mit-ssl-in-dmz)
