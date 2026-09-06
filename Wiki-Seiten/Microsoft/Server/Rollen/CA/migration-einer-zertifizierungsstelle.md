---
title: Migration einer Zertifizierungsstelle
description: Beschreibt den allgemeinen Migrationsvorgang einer Microsoft CA
published: true
date: 2026-09-06T11:59:41.008Z
tags: ca, pki, certificates, migration, zertifikate
editor: markdown
dateCreated: 2023-12-31T14:30:54.360Z
---

# 1. Ziel und Rahmenbedingungen

Bei dieser Migration werden übernommen:

1. Name der Zertifizierungsstelle
2. CA-Zertifikat
3. Privater Schlüssel der SubCA
4. Datenbank der ausgestellten und gesperrten Zertifikate
5. Ausstehende Zertifikatanforderungen
6. CA-Registry-Konfiguration
7. CDP- und AIA-Konfiguration
8. Audit- und Sicherheitskonfiguration (soweit in der Registry enthalten)
9. Ausgestellte Zertifikatvorlagen bei einer Enterprise-CA

Die Root-CA muss dabei normalerweise **kein neues SubCA-Zertifikat ausstellen**, weil Zertifikat und Schlüssel der vorhandenen SubCA unverändert übernommen werden.

> **Wichtig:** Quell- und Ziel-CA dürfen niemals gleichzeitig aktiv sein. Beide würden mit derselben CA-Identität und demselben privaten Schlüssel arbeiten.
{.is-warning}


---

# 2. Entscheidung zum Servernamen

Der **CA-Name bleibt zwingend unverändert**. Der Windows-Servername kann grundsätzlich gleich bleiben oder geändert werden.

## 2.1 Gleicher Servername

**Beispiel**

- Alter Server: `PKI-SUBCA01`
- Neuer Server: `PKI-SUBCA01`
- CA-Name: `Firma Issuing CA 01`

Dies ist die einfachste Variante, besonders wenn CDP- oder AIA-Adressen den Servernamen enthalten.

Dafür muss:

1. Der alte Server aus der Domäne entfernt oder dauerhaft vom Netzwerk getrennt werden.
2. Das alte Computerkonto bereinigt werden.
3. Der neue Server den bisherigen Namen erhalten.
4. Der neue Server anschließend in die Domäne aufgenommen werden.

## 2.2 Neuer Servername

**Beispiel**

- Alter Server: `PKI-SUBCA01`
- Neuer Server: `PKI-SUBCA02`
- CA-Name: `Firma Issuing CA 01`

Dann müssen insbesondere folgende Punkte geprüft werden:

- `CAServerName` in der exportierten Registry-Konfiguration
- CDP-Pfade
- AIA-Pfade
- Dateifreigaben für CRL- und CA-Zertifikatsveröffentlichung
- DNS-Namen in bereits ausgestellten Zertifikaten
- Zugriffsrechte auf Veröffentlichungsverzeichnisse

Bereits ausgestellte Zertifikate behalten ihre bisherigen CDP- und AIA-Adressen. Enthalten diese den alten Servernamen, muss dieser Name beziehungsweise der bisherige HTTP-Pfad weiterhin erreichbar bleiben.

### Empfehlung

Wenn keine organisatorischen Gründe dagegen sprechen, sollte bei einer reinen Servermigration der **bisherige Servername wiederverwendet** werden. Dadurch reduziert sich das Risiko, dass alte CDP-, AIA-, UNC- oder LDAP-Bezüge angepasst werden müssen.

---

# 3. Platzhalter der Anleitung

Die folgenden Platzhalter müssen an die eigene Umgebung angepasst werden:

| Platzhalter | Bedeutung |
|------------|------------|
| `<ALTER-SERVER>` | Alter Servername |
| `<NEUER-SERVER>` | Neuer Servername |
| `<CA-NAME>` | Vollständiger Name der SubCA |
| `<BACKUP-PFAD>` | Sicherungsverzeichnis |
| `<CA-BACKUP-KENNWORT>` | Sicheres Kennwort für den exportierten privaten Schlüssel |

## Beispiel

Lokaler Sicherungspfad:

```
D:\CA-Migration
```

Verzeichnisstruktur:

```
D:\CA-Migration
├── CA-Backup
│   ├── CA-Name.p12
│   └── Database
├── CA-Configuration.reg
├── CA-Templates.txt
├── CA-Config.txt
├── CAPolicy.inf
└── CertEnroll
```

> Der Sicherungsordner enthält den privaten Schlüssel der SubCA und muss entsprechend geschützt werden.

---

# 4. Phase 1: Bestandsaufnahme auf der alten SubCA

Noch keine Änderungen durchführen.

## 4.1 Server- und CA-Informationen erfassen

Administrative Eingabeaufforderung öffnen:

```powershell
hostname
whoami
certutil.exe -getreg CA\CommonName
certutil.exe -getreg CA\CAType
certutil.exe -getreg CA\CAServerName
certutil.exe -getreg CA\DBDirectory
certutil.exe -getreg CA\DBLogDirectory
certutil.exe -getreg CA\DBSystemDirectory
certutil.exe -getreg CA\DBTempDirectory
```

Gesamte CA-Konfiguration dokumentieren:

```powershell
certutil.exe -getreg CA > D:\CA-Migration\CA-Config.txt
```

Dienststatus prüfen:

```powershell
Get-Service -Name CertSvc
```

Installierte AD-CS-Rollendienste dokumentieren:

```powershell
Get-WindowsFeature ADCS*
```

Beispiele zusätzlicher AD-CS-Rollendienste:

- Zertifizierungsstellen-Webregistrierung
- Online Responder
- Network Device Enrollment Service
- Certificate Enrollment Web Service
- Certificate Enrollment Policy Web Service

---

## 4.2 CA-Zertifikat kontrollieren

```powershell
certutil.exe -ca.cert D:\CA-Migration\SubCA.cer
certutil.exe -dump D:\CA-Migration\SubCA.cer
```

Folgende Werte dokumentieren:

1. Subject
2. Issuer
3. Seriennummer
4. Gültig ab
5. Gültig bis
6. Signaturalgorithmus
7. Öffentlicher Schlüssel
8. Schlüsselgröße
9. CDP
10. AIA

Zusätzlich:

```powershell
certutil.exe -store My
```

Die Seriennummer und der Fingerabdruck des CA-Zertifikats werden später mit dem Zielserver verglichen.

---

## 4.3 Kryptografieanbieter und Schlüssel prüfen

```powershell
certutil.exe -getreg CA\CSP
```

Alternativ ausführlicher:

```powershell
certutil.exe -getreg CA\CSP > D:\CA-Migration\CA-CSP.txt
```

Dabei insbesondere erfassen:

- Provider
- ProviderType
- CNG- oder Legacy-CSP
- Hashalgorithmus
- Schlüsselcontainer
- Key Storage Provider
- HSM-Nutzung

> Wird ein HSM verwendet, reicht ein normaler P12-Export möglicherweise nicht aus. Dann ist das Migrationsverfahren des HSM-Herstellers maßgeblich.

---

## 4.4 Zertifikatvorlagen dokumentieren

```powershell
certutil.exe -catemplates > D:\CA-Migration\CA-Templates.txt
```

Datei kontrollieren:

```powershell
Get-Content D:\CA-Migration\CA-Templates.txt
```

Zusätzlich empfiehlt sich ein Screenshot des Knotens:

```
Zertifizierungsstelle
└── <CA-NAME>
    └── Zertifikatvorlagen
```

---

## 4.5 CDP- und AIA-Konfiguration sichern

```powershell
certutil.exe -getreg CA\CRLPublicationURLs > D:\CA-Migration\CRLPublicationURLs.txt
certutil.exe -getreg CA\CACertPublicationURLs > D:\CA-Migration\CACertPublicationURLs.txt
```

PKI-Unternehmensansicht starten:

```powershell
pkiview.msc
```

Prüfen:

- CA-Zertifikate erreichbar
- Basis-CRL erreichbar
- Delta-CRL erreichbar (falls verwendet)
- AIA-Pfade erreichbar
- CDP-Pfade erreichbar
- Keine abgelaufenen CRLs
- Kein Status `Unable to Download`

---

## 4.6 CAPolicy.inf sichern

Prüfen:

```powershell
Test-Path C:\Windows\CAPolicy.inf
```

Falls vorhanden:

```powershell
Copy-Item C:\Windows\CAPolicy.inf D:\CA-Migration\CAPolicy.inf
```

---

## 4.7 CertEnroll-Verzeichnis sichern

```powershell
Copy-Item C:\Windows\System32\CertSrv\CertEnroll D:\CA-Migration\CertEnroll -Recurse
```

Falls CRLs oder CA-Zertifikate über andere Verzeichnisse, Freigaben oder Webserver veröffentlicht werden, diese ebenfalls sichern und dokumentieren.

---

## 4.8 Registry-Konfiguration sichern

```powershell
reg.exe export "HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration" "D:\CA-Migration\CA-Configuration.reg" /y
```

Sicherung prüfen:

```powershell
Test-Path D:\CA-Migration\CA-Configuration.reg
```

Optional:

```powershell
reg.exe export "HKLM\SYSTEM\CurrentControlSet\Services\CertSvc" "D:\CA-Migration\CertSvc-Complete.reg" /y
```

---

# 5. Phase 2: CRL für das Migrationsfenster vorbereiten

## 5.1 Aktuelle Werte dokumentieren

```powershell
certutil.exe -getreg CA\CRLPeriod
certutil.exe -getreg CA\CRLPeriodUnits
certutil.exe -getreg CA\CRLDeltaPeriod
certutil.exe -getreg CA\CRLDeltaPeriodUnits
```

Beispiel:

```
CRLPeriod = Days
CRLPeriodUnits = 7
```

## 5.2 Zeitraum vorübergehend verlängern

```powershell
certutil.exe -setreg CA\CRLPeriodUnits 14
certutil.exe -setreg CA\CRLPeriod Days

Restart-Service CertSvc

certutil.exe -crl
```

## 5.3 Veröffentlichung prüfen

```powershell
pkiview.msc
```

Zusätzlich:

```powershell
certutil.exe -dump C:\Windows\System32\CertSrv\CertEnroll\<CRL-DATEI>.crl
```

Prüfen:

- `ThisUpdate`
- `NextUpdate`
- CRL-Nummer
- Delta-CRL-Status
- Erreichbarkeit über HTTP oder LDAP

---

# 6. Phase 3: Finale Sicherung der alten SubCA

## 6.1 Zertifikatsdienste anhalten

```powershell
Stop-Service CertSvc
Set-Service CertSvc -StartupType Disabled
Get-Service CertSvc
```

## 6.2 CA-Datenbank und privaten Schlüssel sichern

GUI-Methode:

1. `certsrv.msc` öffnen
2. Rechtsklick auf die CA
3. **Alle Aufgaben**
4. **Zertifizierungsstelle sichern**
5. Beide Optionen auswählen:
   - Privater Schlüssel und CA-Zertifikat
   - Zertifikatdatenbank und Zertifikatdatenbank-Protokoll
6. Sicherungspfad festlegen
7. Starkes Kennwort vergeben

Erwartete Struktur:

```
D:\CA-Migration\CA-Backup\<CA-NAME>.p12
D:\CA-Migration\CA-Backup\Database\certbkxp.dat
D:\CA-Migration\CA-Backup\Database\<CA-NAME>.edb
```

## 6.3 Sicherung kontrollieren

```powershell
Get-ChildItem D:\CA-Migration\CA-Backup -Recurse
```

```powershell
certutil.exe -dump D:\CA-Migration\CA-Backup\<CA-NAME>.p12
```

## 6.4 Registry erneut exportieren

```powershell
reg.exe export "HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration" "D:\CA-Migration\CA-Configuration-Final.reg" /y
```

---

# 7. Phase 4: Alten Server außer Betrieb nehmen

Rolle entfernen:

```powershell
Uninstall-WindowsFeature ADCS-Cert-Authority
```

Alternativ:

- Dienst deaktivieren
- Server herunterfahren
- Server keinesfalls parallel zur neuen CA starten

---

# 8. Phase 5: Zielserver vorbereiten

## 8.1 Betriebssystem vorbereiten

- Aktuellen Patchstand installieren
- Statische IP konfigurieren
- Zeitsynchronisation prüfen
- DNS prüfen
- Server härten
- Virenschutz-Ausnahmen prüfen
- Datenbank- und Loglaufwerke vorbereiten

## 8.2 Servernamen festlegen

```powershell
Rename-Computer -NewName "<ALTER-SERVER>" -Restart
```

```powershell
Add-Computer -DomainName "<DOMAENE>" -Restart
```

## 8.3 Backup bereitstellen

```powershell
Copy-Item \\DATEISERVER\CA-Migration D:\CA-Migration -Recurse
```

---

# 9. Phase 6: AD-CS-Rolle installieren

```powershell
Install-WindowsFeature ADCS-Cert-Authority -IncludeManagementTools
```

```powershell
Get-WindowsFeature ADCS-Cert-Authority
```

---

# 10. Phase 7: Ziel-CA mit bestehendem Schlüssel konfigurieren

Im Assistenten:

- Untergeordnete Zertifizierungsstelle auswählen
- Vorhandenen privaten Schlüssel verwenden
- P12-Datei importieren
- Vorhandenes CA-Zertifikat auswählen
- CA-Namen kontrollieren
- Datenbankpfade festlegen

> Keinen neuen Schlüssel erzeugen und keine neue Anforderung an die Root-CA senden.

---

# 11. Phase 8: Registry-Konfiguration importieren

## Identischer Servername

```powershell
Stop-Service CertSvc
reg.exe import D:\CA-Migration\CA-Configuration-Final.reg
```

## Geänderter Servername

Anpassen:

```text
"CAServerName"="<NEUER-SERVER>"
```

Danach:

```powershell
reg.exe import D:\CA-Migration\CA-Configuration-Final.reg
```

---

# 12. Phase 9: CA-Datenbank wiederherstellen

Über:

```
certsrv.msc
→ Alle Aufgaben
→ Zertifizierungsstelle wiederherstellen
```

Wiederherstellen:

- Zertifikatdatenbank
- Zertifikatdatenbank-Protokoll

---

# 13. Phase 10: Zertifikatvorlagen kontrollieren

```powershell
certutil.exe -catemplates
```

Vergleich:

```powershell
Compare-Object (Get-Content D:\CA-Migration\CA-Templates.txt) (certutil.exe -catemplates)
```

---

# 14. Phase 11: CertSvc starten

```powershell
Set-Service CertSvc -StartupType Automatic
Start-Service CertSvc
```

```powershell
Get-Service CertSvc
```

```powershell
certutil.exe -getreg CA\CommonName
certutil.exe -getreg CA\CAServerName
certutil.exe -getreg CA\CAType

certutil.exe -ping
certutil.exe -config - -ping
```

---

# 15. Phase 12: CA-Identität vergleichen

Vergleichen:

- Subject
- Issuer
- Seriennummer
- Fingerabdruck
- Gültigkeit
- Öffentlicher Schlüssel

---

# 16. Phase 13: CRL und AIA testen

```powershell
certutil.exe -crl
```

```powershell
pkiview.msc
```

Zusätzlich:

```powershell
certutil.exe -url <BESTEHENDES-ZERTIFIKAT>.cer
```

---

# 17. Phase 14: Testzertifikat ausstellen

```powershell
gpupdate.exe /force
certutil.exe -pulse
```

Prüfen:

- Seriennummer
- Vorlage
- Antragsteller
- Ereignisprotokolle

---

# 18. Phase 15: Ereignisprotokolle kontrollieren

```powershell
Get-WinEvent -LogName Application
```

Auf folgende Fehler achten:

- Privater Schlüssel nicht verfügbar
- Datenbankpfad nicht erreichbar
- Veröffentlichungsverzeichnis nicht erreichbar
- CRL-Veröffentlichung fehlgeschlagen
- LDAP-Veröffentlichung fehlgeschlagen
- Zertifikatvorlagenfehler
- RPC/DCOM-Fehler

---

# 19. Phase 16: CRL-Zeitraum zurückstellen

```powershell
certutil.exe -setreg CA\CRLPeriodUnits 7
certutil.exe -setreg CA\CRLPeriod Days

Restart-Service CertSvc

certutil.exe -crl
```

---

# 20. Nacharbeiten

## Backup der migrierten CA

- CA-Zertifikat und privater Schlüssel
- CA-Datenbank und Logs
- Registry-Konfiguration
- `CAPolicy.inf`
- Zertifikatvorlagenliste
- CertEnroll-Verzeichnis
- Dokumentation der CDP- und AIA-Pfade

## Backup-Dateien schützen

- Nicht unverschlüsselt speichern
- Zugriff protokollieren
- Offline archivieren
- Kennwort getrennt aufbewahren
- Temporäre Kopien entfernen

---

# 21. Rückfallplan

1. Zielserver herunterfahren
2. Netzwerkverbindung trennen
3. Alten Server reaktivieren oder wiederherstellen
4. CertSvc starten
5. CRL-Funktion prüfen
6. Keine parallele Zertifikatsausstellung

> Ein VM-Snapshot ist kein Ersatz für eine vollständige CA-Sicherung.

---

# 22. Abschließende Checkliste

## Vor der Migration

- [ ] CA-Typ dokumentiert
- [ ] CA-Name dokumentiert
- [ ] CA-Zertifikat dokumentiert
- [ ] Fingerabdruck dokumentiert
- [ ] Kryptografieanbieter dokumentiert
- [ ] HSM-Nutzung geklärt
- [ ] Datenbankpfade dokumentiert
- [ ] CDP/AIA dokumentiert
- [ ] PKIView fehlerfrei
- [ ] Zertifikatvorlagen exportiert
- [ ] `CAPolicy.inf` gesichert
- [ ] Registry exportiert
- [ ] Verlängerte CRL veröffentlicht
- [ ] CA-Datenbank gesichert
- [ ] Sicherung geprüft
- [ ] Rückfallplan vorhanden

## Während der Migration

- [ ] CertSvc auf der Quelle beendet
- [ ] Quelle nicht mehr aktiv
- [ ] Alte CA-Rolle entfernt oder isoliert
- [ ] Neuer Server vorbereitet
- [ ] AD-CS-Rolle installiert
- [ ] Vorhandenen Schlüssel importiert
- [ ] Vorhandenes CA-Zertifikat übernommen
- [ ] CA-Name unverändert
- [ ] Registry importiert
- [ ] `CAServerName` angepasst (falls erforderlich)
- [ ] Datenbank wiederhergestellt
- [ ] Zertifikatvorlagen kontrolliert

## Nach der Migration

- [ ] CA-Zertifikat identisch
- [ ] Privater Schlüssel verfügbar
- [ ] CA-Dienst läuft
- [ ] Datenbank verfügbar
- [ ] CRL-Veröffentlichung funktioniert
- [ ] AIA/CDP erreichbar
- [ ] Zertifikatsprüfung erfolgreich
- [ ] Testzertifikat ausgestellt
- [ ] Autoenrollment getestet
- [ ] Sperrung getestet
- [ ] Ereignisprotokolle geprüft
- [ ] CRL-Zeitraum zurückgesetzt
- [ ] Finale Sicherung erstellt
- [ ] Alte CA weiterhin ausgeschaltet und isoliert


# Quelle:
Hier noch eine alte Quelle aus dem Technet, die Anleitung oben beruht nur noch Wage darauf und ist eher als Archiv zu betrachten.
http://technet.microsoft.com/en-us/library/ee126170(v=ws.10).aspx