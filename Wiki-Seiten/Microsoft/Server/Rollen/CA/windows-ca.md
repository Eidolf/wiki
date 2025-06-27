---
title: windows-ca
description: 
published: true
date: 2025-06-27T15:24:36.546Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:50.407Z
---

# Windows CA

## Einrichten

### Server Core

> **Eine Warnung!** Nach längerem Arbeiten mit einer Server Core Root CA würde ich von so einer Installation abraten, die Root CA sollte mindestens die GUI enthalten, besser auch die Sub CA
{.is-warning}


1. Grundinstallation und Einstellungen einer Server Core Installation  
[Windows-Server-Core](/de/Wiki-Seiten/Microsoft/Server/windows-server-core)
2. Stamm Zertifizierungsstelle installieren und einrichten
[root-ca-installieren](/de/Wiki-Seiten/Microsoft/Server/Rollen/CA/root-ca-installieren)
3. Weitere Konfigurationen und Einstellungen  
[root-ca-konfigurieren](/de/Wiki-Seiten/Microsoft/Server/Rollen/CA/root-ca-konfigurieren)
4. Enterprise CA (Sub CA) installieren und konfigurieren
[enterprise-ca-installieren](/de/Wiki-Seiten/Microsoft/Server/Rollen/CA/enterprise-ca-installieren)
[enterprise-ca-konfigurieren](/de/Wiki-Seiten/Microsoft/Server/Rollen/CA/enterprise-ca-konfigurieren)

## Standard Aufgaben

### Sub CA Zertifikat erneuern

#### Im Server Core Umfeld

Eines der Punkte weshalb ich eine Warnung bei Server Core Installationen hinzugefügt habe ist der umständliche Weg das Sub CA Zertifikat zu erneuern.  
Es werden leider wichtige Punkte in der Remote MMC ausgeblendet und weiterhin ist es nicht einfach Berechtigungen zu übergeben.  
Berechtigung ist der nächste wichtige Punkt, um einfach mit certsrv.msc eine Remoteverbindung mit der Root CA herzustellen (wird für das Ausstellen benötigt), sollte das Passwort des lokalen Admins an der Root CA gleich sein wie mit dem lokalen Admin des Remoteclients.

1. Shell Verbindung zur Sub CA herstellen
2. Befehl `certutil.exe -renewcert` ausführen, hiermit wird auch der private Schlüssel getauscht. Falls hier ein Fehler erscheint kann es sein das schon ein Versuch gestartet wurde. Hier kann man mit `certutil.exe -renewcert -f` die bestehende Anforderung überschreiben.
![SubCARenewCert001.png](/media/SubCARenewCert001.png)
3. Da keine Verbindung zur Root CA hergestellt werden kann wird man zum Schluss auf Abbrechen drücken müssen und die Requestdatei manuell an die Root CA einreichen. Die Request Datei liegt auf der Sub CA direkt unter C:\\
4. certsrv.msc Verbindung zur Root CA herstellen (Wie gesagt lokales Administrator Passwort beachten).
5. Rechtklick auf den Verbindungspunkt der Root CA &gt; Alle Aufgaben &gt; Neue Anforderung einreichen
![SubCARenewCert002.png](/media/SubCARenewCert002.png)
6. Die Request Datei auswählen
![SubCARenewCert003.png](/media/SubCARenewCert003.png)
7. Unter den Ausstehenden Anforderungen den Request mit Rechtklick > Alle Aufgaben > Ausstellen freigeben
![SubCARenewCert004.png](/media/SubCARenewCert004.png)
8. Unter Ausgestellte Zertifikate das neue Zertifikat exportieren mit doppelklick auf das Zertifikat > Details > In Datei kopieren > .p7b export inkl. aller Zertifikate
![install-enterprise-ca_027.png](/media/install-enterprise-ca_027.png)
9. Shell Verbindung zur Sub CA herstellen
10. Mit dem Befehl `certutil -installcert c:\CertDateiName.p7b` das Zertifikat installieren.
![install-enterprise-ca_042-01.png](/media/install-enterprise-ca_042-01.png)
11. Den Certsrv Dienst durchstarten

## Erweiterte Aufgabe

### SAN Zertifikat erstellen

#### IIS Wizard (certsrv)

Unter dem Attribut Feld gibt man SAN:DNS=SAN.FQDN ein.  
Jeder weitere Eintrag wird durch ein kaufmännisches UND "&amp;" getrennt

Beispiel:

`
SAN:DNS=eins.domain.de & DNS=zwei.domain.de
`

## Einstellungen

### Bestehende SHA1 CA auf SHA256 upgraden

1. CMD öffnen
2. `certutil -setreg ca\csp\CNGHashAlgorithm SHA256`
3. CA Dienst neustarten
4. Eigenschaften der CA überprüfen
5. CA Zertifikat erneuern

#### Quelle:

https://www.frankysweb.de/migration-stammzertifizierungsstelle-sha1-zu-sha256-hashalgorithmus/

### Zertifikat kann nicht unter "Ausstellende Zertifikatsvorlage" ausgewählt werden

Lösung:

1. ADSI Edit öffnen
2. Auf `CN=Configuration | CN=Services | CN=Public Key Services | CN=Enrollment Services` wechseln
3. Eigenschaften der Zertifizierungsstelle auswählen, bei der die Vorlagen nicht ausgewählt werden können
4. Attribut **flag** von **2** auf **10** setzen
5. Dienst der betroffenen CA neu starten

#### Quelle:

https://serverfault.com/questions/320565/certificate-template-missing-from-certificate-template-to-issue