---
title: Defekten Domänencontroller aus AD entfernen
description: Anleitung zum entfernen eines defekten Domänencontrollers aus dem Active Directory
published: true
date: 2026-09-03T16:19:15.829Z
tags: ad, dc, domain controller, korrupt, defekt
editor: markdown
dateCreated: 2026-09-03T15:23:16.739Z
---

# Nicht mehr verfügbaren Domänencontroller aus Active Directory entfernen

Diese Anleitung beschreibt das sichere Entfernen eines Domänencontrollers, der dauerhaft ausgefallen ist, nicht mehr gestartet werden kann oder physisch beziehungsweise virtuell nicht mehr existiert. Der Vorgang wird als **erzwungene Entfernung mit Metadatenbereinigung** bezeichnet.

> **Wichtig:** Führen Sie diese Schritte nur aus, wenn der ausgefallene Domänencontroller definitiv nicht mehr normal gestartet und herabgestuft werden kann. Nach der Bereinigung darf die alte Installation des Domänencontrollers nicht wieder mit dem Netzwerk verbunden werden.
{.is-warning}


# 1. Voraussetzungen und wichtige Sicherheitsprüfung

Für die Arbeiten benötigen Sie normalerweise:

- Ein Konto mit Mitgliedschaft in **Domänen-Admins**
- Für Änderungen an gesamtstrukturweiten FSMO-Rollen gegebenenfalls **Organisations-Admins**
- Zugriff auf mindestens einen funktionierenden, beschreibbaren Domänencontroller
- Die Active-Directory-Verwaltungstools beziehungsweise RSAT
- Eine administrative PowerShell oder Eingabeaufforderung

## Entscheidend: Existiert noch ein funktionierender Domänencontroller?

Führen Sie die Metadatenbereinigung nur aus, wenn mindestens ein anderer Domänencontroller der Domäne ordnungsgemäß funktioniert.

Falls der ausgefallene Server der **einzige Domänencontroller** der Domäne war, darf er nicht einfach aus Active Directory entfernt werden. In diesem Fall benötigen Sie eine Wiederherstellung des Domänencontrollers beziehungsweise der Gesamtstruktur aus einer geeigneten Systemstatussicherung.

## Vorhandene Domänencontroller anzeigen

Auf einem funktionierenden Domänencontroller oder einem Verwaltungsrechner mit installiertem Active-Directory-Modul:

```powershell
Get-ADDomainController -Filter * |
    Select-Object HostName, Site, IPv4Address, IsGlobalCatalog, OperationMasterRoles
```

Prüfen Sie zusätzlich, ob der noch vorhandene Domänencontroller die Domäne lesen und schreiben kann:

```powershell
Get-ADDomain
Get-ADForest
```

# 2. Zustand der Replikation prüfen

Öffnen Sie eine Eingabeaufforderung oder PowerShell als Administrator:

```cmd
repadmin /replsummary
```

Anschließend:

```cmd
repadmin /showrepl *
```

Der ausgefallene Domänencontroller wird wahrscheinlich mit Replikationsfehlern angezeigt. Entscheidend ist, dass die verbleibenden Domänencontroller untereinander erfolgreich replizieren.

Eine ausführlichere Übersicht kann mit folgendem Befehl erzeugt werden:

```cmd
repadmin /showrepl * /csv
```

Prüfen Sie außerdem die Erreichbarkeit der Domänendienste:

```cmd
dcdiag /e /c /v
```

Für eine gezielte Prüfung des DNS-Dienstes:

```cmd
dcdiag /test:dns /e /v
```

> Es ist nicht notwendig, dass alle Tests bezüglich des bereits ausgefallenen Domänencontrollers erfolgreich sind. Fehler zwischen den verbleibenden Domänencontrollern müssen jedoch vor oder unmittelbar nach der Bereinigung untersucht werden.

# 3. FSMO-Rollen prüfen

Ermitteln Sie, welche Domänencontroller aktuell die FSMO-Rollen besitzen:

```cmd
netdom query fsmo
```

Alternativ mit PowerShell:

```powershell
Get-ADDomain | Select-Object PDCEmulator, RIDMaster, InfrastructureMaster
Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster
```

Die fünf FSMO-Rollen sind:

- Schemamaster
- Domänennamenmaster
- PDC-Emulator
- RID-Master
- Infrastrukturmaster

## Falls der ausgefallene Domänencontroller FSMO-Rollen besitzt

Wenn der alte Domänencontroller definitiv nicht zurückkehren wird, müssen die betroffenen Rollen auf einen funktionierenden Domänencontroller **erzwungen übertragen**, also übernommen, werden.

Ersetzen Sie `DC02` durch den Namen des funktionierenden Ziel-Domänencontrollers.

Beispiel für die Übernahme aller Rollen:

```powershell
Move-ADDirectoryServerOperationMasterRole `
    -Identity "DC02" `
    -OperationMasterRole SchemaMaster,DomainNamingMaster,PDCEmulator,RIDMaster,InfrastructureMaster `
    -Force
```

Übernehmen Sie möglichst nur die Rollen, die tatsächlich auf dem ausgefallenen Server lagen. Beispiel für PDC-Emulator und RID-Master:

```powershell
Move-ADDirectoryServerOperationMasterRole `
    -Identity "DC02" `
    -OperationMasterRole PDCEmulator,RIDMaster `
    -Force
```

Danach erneut prüfen:

```cmd
netdom query fsmo
```

> Eine mit `-Force` übernommene FSMO-Rolle darf nicht wieder gleichzeitig vom alten Domänencontroller angeboten werden. Der alte Domänencontroller muss dauerhaft offline bleiben oder vor einer erneuten Verwendung vollständig neu installiert werden.

# 4. Prüfen, ob der ausgefallene Server zusätzliche Rollen hatte

Vor der Entfernung sollten Sie feststellen, ob der Server neben Active Directory weitere wichtige Aufgaben erfüllte:

- DNS-Server
- Globaler Katalog
- DHCP-Server
- Zertifizierungsstelle
- NPS beziehungsweise RADIUS
- Dateiserver oder DFS-Namespace
- DFS-Replikation außerhalb von SYSVOL
- Windows-Zeitquelle
- ADFS oder Web Application Proxy
- Entra-Connect-Synchronisierung
- Anwendungen mit fest eingetragenem LDAP-Server
- Backup-, Monitoring- oder Verwaltungsserver

## Globalen Katalog prüfen

```powershell
Get-ADDomainController -Filter * |
    Select-Object HostName, IsGlobalCatalog, Site
```

In jedem Standort sollte nach Möglichkeit ein erreichbarer globaler Katalog vorhanden sein. Dieser muss sich jedoch nicht zwingend in derselben Site befinden. Ein Domänencontroller gehört immer genau zu der Site, in der sich sein Serverobjekt befindet, und sollte nicht zur Abdeckung einer anderen Site verschoben oder mehrfach zugewiesen werden.

Falls erforderlich, kann ein verbleibender Domänencontroller als globaler Katalog konfiguriert werden:

1. Öffnen Sie `dssite.msc`.
2. Navigieren Sie zu **Standorte**.
3. Öffnen Sie den Standort und anschließend **Servers**.
4. Öffnen Sie den funktionierenden Domänencontroller.
5. Klicken Sie mit der rechten Maustaste auf **NTDS Settings**.
6. Wählen Sie **Eigenschaften**.
7. Aktivieren Sie **Globaler Katalog**.

Besitzt eine Site keinen eigenen Domänencontroller beziehungsweise globalen Katalog, verwenden die Clients automatisch einen erreichbaren Server aus einer anderen Site. Prüfen in diesem Fall:

- Das Client-IP-Netz ist unter **Subnets** der richtigen Site zugeordnet.
- Die Site ist über einen geeigneten **Site Link** angebunden.
- Die Site-Link-Kosten bevorzugen den gewünschten Standort.
- Der erreichbare Domänencontroller ist als globaler Katalog konfiguriert.

Die Auswahl kann auf einem Client der betroffenen Site geprüft werden:

```cmd
nltest /dsgetsite
nltest /dsgetdc:deinedomaene.local /force
nltest /dsgetdc:deinedomaene.local /GC /force
```

Folgender Befehl ist für die Forest Root Domain Prüfung, die eventuell unterschiedlich ist.
```cmd
nltest /dsgetdc:deinerootdomaene.local /GC /force
```

**Kurz gesagt:** Den Domänencontroller nicht verschieben oder doppelt zuweisen, sondern Subnetze, Site Links und deren Kosten korrekt konfigurieren.


# 5. Metadaten über Active Directory-Benutzer und -Computer entfernen

Dies ist bei aktuellen Windows-Server-Versionen der bevorzugte Weg.

1. Melden Sie sich an einem funktionierenden Domänencontroller oder Verwaltungsrechner an.
2. Öffnen Sie **Active Directory-Benutzer und -Computer**:

```cmd
dsa.msc
```

3. Aktivieren Sie unter **Ansicht** die Option **Erweiterte Features**.
4. Öffnen Sie die Organisationseinheit **Domain Controllers**.
5. Suchen Sie das Computerkonto des ausgefallenen Domänencontrollers.
6. Klicken Sie mit der rechten Maustaste darauf und wählen Sie **Löschen**.
7. Bestätigen Sie die Warnung.
8. Wenn angezeigt, aktivieren Sie die Option sinngemäß:

   **Dieser Domänencontroller ist dauerhaft offline und kann nicht mehr mit dem Active Directory-Installationsassistenten herabgestuft werden.**

9. Bestätigen Sie die Entfernung.

Bei unterstützten Verwaltungstools entfernt dieser Vorgang normalerweise gleichzeitig die zugehörigen Domänencontroller-Metadaten einschließlich des NTDS-Settings-Objekts.

# 6. Einträge in Active Directory-Standorte und -Dienste kontrollieren

Öffnen Sie:

```cmd
dssite.msc
```

Navigieren Sie zu:

```text
Standorte
  <Name des Standorts>
    Servers
      <Name des ausgefallenen Domänencontrollers>
```

Prüfen Sie, ob der alte Server noch vorhanden ist.

## Falls noch ein NTDS-Settings-Objekt vorhanden ist

1. Klicken Sie mit der rechten Maustaste auf **NTDS Settings**.
2. Wählen Sie **Löschen**.
3. Aktivieren Sie bei entsprechender Nachfrage die Bestätigung, dass der Domänencontroller dauerhaft offline ist.
4. Löschen Sie anschließend das nun leere Serverobjekt.

> Löschen Sie nicht unüberlegt manuell einzelne Replikationsverbindungen. Entfernen Sie zuerst das NTDS-Settings-Objekt und danach das Serverobjekt des ausgefallenen Domänencontrollers.

# 7. Alternative Metadatenbereinigung mit `ntdsutil`

Verwenden Sie diese Methode, wenn die grafische Bereinigung nicht möglich ist oder verwaiste Metadaten verbleiben.

Öffnen Sie eine Eingabeaufforderung als Administrator:

```cmd
ntdsutil
```

Wechseln Sie zur Metadatenbereinigung:

```cmd
metadata cleanup
```

Stellen Sie eine Verbindung zu einem funktionierenden Domänencontroller her:

```cmd
connections
connect to server DC02
quit
```

Ersetzen Sie `DC02` durch den Namen eines funktionierenden Domänencontrollers.

Wählen Sie anschließend das zu entfernende Serverobjekt aus:

```cmd
select operation target
list domains
select domain <Nummer>
list sites
select site <Nummer>
list servers in site
select server <Nummer>
quit
```

Kontrollieren Sie sehr sorgfältig, dass der ausgewählte Server tatsächlich der ausgefallene Domänencontroller ist. Entfernen Sie ihn anschließend:

```cmd
remove selected server
```

Bestätigen Sie die Rückfragen und beenden Sie das Programm:

```cmd
quit
quit
```

> Verwenden Sie `remove selected server` niemals, bevor Sie anhand der angezeigten Auswahl geprüft haben, dass wirklich der richtige Domänencontroller ausgewählt ist.

# 8. DNS-Einträge bereinigen

Ein ausgefallener Domänencontroller hinterlässt häufig DNS-Einträge. Öffnen Sie den DNS-Manager:

```cmd
dnsmgmt.msc
```

Prüfen Sie die folgenden Bereiche.

## Hosteinträge

Entfernen Sie veraltete Einträge des ausgefallenen Servers:

- A-Eintrag
- AAAA-Eintrag
- Veraltete PTR-Einträge in Reverse-Lookupzonen

## SRV-Einträge

Kontrollieren Sie insbesondere die Ordner:

```text
_ldap
_kerberos
_kpasswd
_gc
_tcp
_udp
_sites
```

Diese befinden sich üblicherweise innerhalb der DNS-Zonen:

```text
<ihre-domäne>
_msdcs.<ihre-gesamtstruktur>
```

Entfernen Sie nur SRV-Einträge, die eindeutig auf den ausgefallenen Domänencontroller verweisen.

## CNAME-Eintrag in `_msdcs`

In der Zone `_msdcs.<Gesamtstruktur-Stammdomäne>` kann ein CNAME-Eintrag mit der DSA-GUID des alten Domänencontrollers existieren. Dieser verweist auf den vollständigen DNS-Namen des ausgefallenen Servers und sollte entfernt werden.

## Nameserver-Einträge

Falls der alte Domänencontroller auch DNS-Server war:

1. Öffnen Sie die Eigenschaften jeder relevanten DNS-Zone.
2. Prüfen Sie die Registerkarte **Nameserver**.
3. Entfernen Sie den ausgefallenen Server aus der Liste.
4. Prüfen Sie Delegierungen in übergeordneten Zonen.
5. Prüfen Sie DNS-Weiterleitungen und bedingte Weiterleitungen.

## DNS-Konfiguration der Clients und Server

Prüfen Sie, ob der ausgefallene DNS-Server noch verteilt oder statisch verwendet wird:

- DHCP-Option 006
- Statische Netzwerkkonfiguration von Servern
- Hypervisoren
- Netzwerkgeräte
- VPN-Konfigurationen
- Anwendungen
- Appliances
- Drucker und Scanner

Auf Windows-Systemen kann die aktuelle DNS-Konfiguration so angezeigt werden:

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4
```

Nach Änderungen kann der lokale DNS-Cache geleert werden:

```cmd
ipconfig /flushdns
```

Der funktionierende Domänencontroller kann seine DNS-Einträge erneut registrieren:

```cmd
ipconfig /registerdns
```

Zusätzlich kann der Netlogon-Dienst zur erneuten Registrierung der domänenspezifischen Einträge neu gestartet werden:

```powershell
Restart-Service Netlogon
```

# 9. Computerobjekt und weitere verwaiste Objekte kontrollieren

Prüfen Sie in **Active Directory-Benutzer und -Computer**, ob das Computerkonto des alten Domänencontrollers vollständig entfernt wurde.

Mit PowerShell:

```powershell
Get-ADComputer -Identity "DC01"
```

Wenn der Befehl meldet, dass das Objekt nicht gefunden wurde, ist das Computerkonto bereits entfernt.

Prüfen Sie zusätzlich, ob noch ein Domänencontrollerobjekt zurückgegeben wird:

```powershell
Get-ADDomainController -Identity "DC01"
```

Ersetzen Sie `DC01` durch den Namen des ausgefallenen Servers.

# 10. Replikation nach der Bereinigung anstoßen

Stoßen Sie die Replikation auf den verbleibenden Domänencontrollern an:

```cmd
repadmin /syncall /AdeP
```

Bedeutung der verwendeten Optionen:

- `/A`: Alle Namenskontexte
- `/d`: Servernamen in den Meldungen anzeigen
- `/e`: Standortübergreifende Replikation einbeziehen
- `/P`: Änderungen nach außen übertragen

Prüfen Sie danach erneut:

```cmd
repadmin /replsummary
```

Und:

```cmd
repadmin /showrepl *
```

Wenn nur noch ein Domänencontroller vorhanden ist, gibt es keinen Replikationspartner. In diesem Fall darf `repadmin` keine erfolgreiche Replikation zu einem zweiten Server erwarten lassen. Es sollten aber keine aktiven Verweise auf den entfernten Server bestehen bleiben.

# 11. Active Directory diagnostizieren

Führen Sie nach der Bereinigung eine Gesamtprüfung aus:

```cmd
dcdiag /e /c /v
```

DNS-Prüfung:

```cmd
dcdiag /test:dns /e /v
```

Prüfung der SYSVOL- und Netlogon-Freigaben:

```cmd
net share
```

Auf jedem verbleibenden Domänencontroller sollten mindestens die folgenden Freigaben vorhanden sein:

```text
NETLOGON
SYSVOL
```

Prüfen Sie außerdem den SYSVOL-Replikationsstatus:

```cmd
dcdiag /test:sysvolcheck /test:advertising
```

# 12. Ereignisprotokolle kontrollieren

Öffnen Sie die Ereignisanzeige:

```cmd
eventvwr.msc
```

Prüfen Sie insbesondere:

- Verzeichnisdienst
- DNS-Server
- DFS-Replikation
- System
- Active Directory Web Services

Achten Sie auf wiederkehrende Meldungen, die weiterhin den entfernten Domänencontroller, dessen GUID, dessen DNS-Namen oder seine IP-Adresse nennen.

Einzelne ältere Ereignisse sind nach der Bereinigung nicht automatisch problematisch. Entscheidend ist, ob neue Fehler weiterhin auftreten.

# 13. Zeitdienst kontrollieren

Wenn der ausgefallene Domänencontroller der PDC-Emulator oder die externe Zeitquelle war, muss die Zeitkonfiguration geprüft werden.

Aktuellen Status anzeigen:

```cmd
w32tm /query /status
```

Konfiguration anzeigen:

```cmd
w32tm /query /configuration
```

Quelle anzeigen:

```cmd
w32tm /query /source
```

Der PDC-Emulator der Gesamtstruktur-Stammdomäne sollte gegen eine zuverlässige externe Zeitquelle synchronisieren. Andere Domänenmitglieder folgen normalerweise der Active-Directory-Zeithierarchie.

# 14. DHCP kontrollieren

Falls der ausgefallene Server DHCP ausführte:

1. Prüfen Sie, ob ein anderer DHCP-Server vorhanden ist.
2. Prüfen Sie die DHCP-Autorisierung in Active Directory.
3. Entfernen Sie gegebenenfalls den alten DHCP-Server aus der Liste autorisierter Server.
4. Stellen Sie sicher, dass DHCP-Option 006 erreichbare DNS-Server verteilt.
5. Prüfen Sie Option 015 für das korrekte DNS-Suffix.

Autorisierte DHCP-Server können mit installiertem DHCP-PowerShell-Modul angezeigt werden:

```powershell
Get-DhcpServerInDC
```

Einen nicht mehr existierenden DHCP-Server nur dann aus der Autorisierung entfernen, wenn Name und IP-Adresse eindeutig stimmen:

```powershell
Remove-DhcpServerInDC -DnsName "DC01.contoso.local" -IPAddress 192.0.2.10
```

Ersetzen Sie Beispielname und Beispieladresse durch die tatsächlichen Werte.

# 15. Verweise in Gruppenrichtlinien und Skripten prüfen

Suchen Sie nach festen Verweisen auf den alten Server, zum Beispiel:

```text
\\DC01\Freigabe
LDAP://DC01
DC01.contoso.local
192.0.2.10
```

Prüfen Sie insbesondere:

- Anmeldeskripte
- Gruppenrichtlinien
- Laufwerkszuordnungen
- Druckerzuordnungen
- Geplante Aufgaben
- Dienstkonten und Dienste
- Monitoring
- Backupsoftware
- Anwendungen mit LDAP- oder LDAPS-Bindung
- Zertifikatvorlagen und Zertifizierungsstellen
- DFS-Namespace-Ziele

Verwenden Sie für Dienste möglichst den Domänennamen oder geeignete hochverfügbare Dienstnamen anstelle eines fest eingetragenen einzelnen Domänencontrollers.

# 16. Besonderheit bei einer Zertifizierungsstelle

War auf dem ausgefallenen Domänencontroller eine Active-Directory-Zertifizierungsstelle installiert, reicht die Domänencontroller-Metadatenbereinigung nicht aus.

In diesem Fall müssen zusätzlich berücksichtigt werden:

- Sicherung oder Wiederherstellung der CA-Datenbank
- Privater Schlüssel der Zertifizierungsstelle
- CA-Zertifikat
- Sperrlisten-Verteilungspunkte
- AIA-Veröffentlichung
- Zertifikatvorlagen
- Verweise in Active Directory
- Automatische Zertifikatsregistrierung
- OCSP-Konfiguration

Löschen Sie CA-Objekte nicht pauschal, wenn noch ausgestellte Zertifikate verwendet oder geprüft werden müssen.

# 17. Alten Domänencontroller niemals unverändert wieder einschalten

Nach einer erzwungenen Entfernung und Metadatenbereinigung gilt:

- Den alten Domänencontroller nicht wieder mit dem Produktivnetz verbinden.
- Keine alte virtuelle Maschine unverändert starten.
- Keinen alten Snapshot als Domänencontroller zurückrollen und produktiv verwenden.
- Keine parallele Installation mit demselben Computerkonto betreiben.

Wenn die Hardware oder virtuelle Maschine wiederverwendet werden soll:

1. Netzwerkverbindung zunächst trennen.
2. Betriebssystem neu installieren oder die Maschine sicher auf einen Zustand vor der Domänencontrollerrolle zurücksetzen.
3. Sicherstellen, dass keine alte Active-Directory-Datenbank mehr verwendet wird.
4. Einen eindeutigen Computernamen und eine korrekte IP-Konfiguration festlegen.
5. Den Server neu in die Domäne aufnehmen.
6. Bei Bedarf erneut ordnungsgemäß zum Domänencontroller heraufstufen.

# 18. Falls der alte Server wider Erwarten wieder verfügbar wird

Wenn die Metadatenbereinigung bereits durchgeführt oder FSMO-Rollen erzwungen übernommen wurden, darf der alte Domänencontroller nicht normal wieder in Betrieb genommen werden.

Die sichere Vorgehensweise ist:

1. Server vom Netzwerk getrennt halten.
2. Benötigte Nicht-AD-Daten nur über ein kontrolliertes Verfahren sichern.
3. Betriebssystem neu installieren.
4. Server erneut als Mitgliedsserver aufnehmen.
5. Bei Bedarf neu zum Domänencontroller heraufstufen.

# 19. Abschlusskontrolle

Die Bereinigung ist erfolgreich, wenn alle folgenden Punkte erfüllt sind:

- Der ausgefallene Server erscheint nicht mehr in der OU **Domain Controllers**.
- Er erscheint nicht mehr unter **Active Directory-Standorte und -Dienste**.
- `Get-ADDomainController -Filter *` zeigt ihn nicht mehr an.
- `netdom query fsmo` zeigt nur erreichbare FSMO-Rolleninhaber.
- DNS enthält keine relevanten A-, AAAA-, CNAME-, NS- oder SRV-Einträge des alten Servers.
- DHCP verteilt keine alte DNS-Serveradresse.
- Die verbleibenden Domänencontroller replizieren fehlerfrei.
- `dcdiag` meldet keine aktuellen kritischen Fehler bezüglich des entfernten Servers.
- SYSVOL und NETLOGON sind auf den verbleibenden Domänencontrollern verfügbar.
- Anwendungen verwenden keinen fest eingetragenen Verweis auf den entfernten Server.
- Der alte Domänencontroller bleibt dauerhaft offline oder wird vollständig neu installiert.

# 20. Kompakte Befehlsübersicht

```powershell
# Vorhandene Domänencontroller
Get-ADDomainController -Filter * |
    Select-Object HostName, Site, IPv4Address, IsGlobalCatalog, OperationMasterRoles

# FSMO-Rollen
Get-ADDomain | Select-Object PDCEmulator, RIDMaster, InfrastructureMaster
Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster

# Beispiel: Erzwungene Übernahme aller FSMO-Rollen
Move-ADDirectoryServerOperationMasterRole `
    -Identity "DC02" `
    -OperationMasterRole SchemaMaster,DomainNamingMaster,PDCEmulator,RIDMaster,InfrastructureMaster `
    -Force

# Kontrolle auf ein altes Computerobjekt
Get-ADComputer -Identity "DC01"

# Kontrolle auf ein altes Domänencontrollerobjekt
Get-ADDomainController -Identity "DC01"

# DNS-Konfiguration
Get-DnsClientServerAddress -AddressFamily IPv4
```

```cmd
netdom query fsmo
repadmin /replsummary
repadmin /showrepl *
repadmin /syncall /AdeP
dcdiag /e /c /v
dcdiag /test:dns /e /v
dcdiag /test:sysvolcheck /test:advertising
ipconfig /flushdns
ipconfig /registerdns
w32tm /query /status
w32tm /query /source
```

# Empfohlene Reihenfolge

1. Sicherstellen, dass mindestens ein funktionierender Domänencontroller vorhanden ist.
2. Replikation und DNS prüfen.
3. FSMO-Rollen ermitteln und gegebenenfalls übernehmen.
4. Abhängigkeiten wie DNS, DHCP, globaler Katalog, Zeitdienst und Zertifizierungsstelle prüfen.
5. Domänencontrollerobjekt über **Active Directory-Benutzer und -Computer** löschen.
6. **Active Directory-Standorte und -Dienste** auf Reste prüfen.
7. Falls erforderlich, `ntdsutil` zur Metadatenbereinigung verwenden.
8. DNS-, DHCP- und Anwendungsverweise bereinigen.
9. Replikation anstoßen.
10. `repadmin`, `dcdiag`, DNS und Ereignisprotokolle kontrollieren.
11. Alte Maschine dauerhaft offline lassen oder vollständig neu installieren.