---
title: Kommandokonsole - CMD
description: 
published: true
date: 2025-08-14T09:47:59.575Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:28:44.725Z
---

# BootRec

Problem mit dem Windows Boot Sektor bereinigen.

1. `bootrec /FixMBR`
2. `bootrec /ScanOS`
3. `bootrec /RebuildBCD`
4. `bootrec /FixBoot`

Falls kein Betriebssystem mehr zu finden ist kann man einen Booteintrag mit folgenden Befehl schreiben.

`bcdboot C:\Windows /s C: /f BIOS`
## Quelle:
https://appuals.com/how-to-fix-bootrec-fixboot-access-denied-on-windows-78-and-10/
https://www.computerwoche.de/a/windows-10-boot-manager-anpassen-entfernen-und-reparieren,2079024,2

# Scripting
[batch-erstellen-cmd-befehle](/de/Wiki-Seiten/Microsoft/Allgemein/batch-erstellen-cmd-befehle)

# Symbolischer Link ( mklink )

1. mklinks werden dazu benötigt Dateien oder auch Pfade Umzuleiten, das sozusagen die Zieldatei die geändert wird von jedem beliebigen Startpunkt bearbeiten kann. Das erste Beispiel ist der Standard mit mklink ohne Option, nämlich eine Datei zu verknüpfen.
`mklink E:\Ausgangsordner\Ausgangsdatei.txt C:\Users\Zielordner\Zieldatei.txt`
2. Mit **/j** erstellt man eine Ordnerweiterleitung. Dies wird primär für eine Umleitung eines Ordners zu einem anderen Pfad verwendet. Dringend nötig auch für Remotezugriff von anderen Rechnern da symbolische Links zu einem Pfad dies nicht können. Weiterhin darf Ausgangsordner nicht vorhanden sein, dieser wird durch den Befehl angelegt.
`mklink /j E:\Ausgangsordner C:\Users\Zielordner`
3. Mit **/d** erstellt man einen symbolischen Link zu einem Pfad, der Standard (Punkt 1) ohne Option ist zu einer Datei, er ist auch ähnlich wie mit Option /j doch ist ein Remotezugriff problematisch.
`mklink /d E:\Ausgangsordner C:\Users\Zielordner`
4. Mit **/h** erstellt man einen Hard link. Hardlinks verknüpfen Dateien in verschiedenen Ordnern aber auf dem gleichen Volume.
`mklink /h C:\Ausgangsordner\Ausgangsdatei.txt C:\Users\Zielordner\Zieldatei.txt`

## Quelle:

https://technet.microsoft.com/de-de/library/cc753194(v=ws.10).aspx
https://superuser.com/questions/343074/directory-junction-vs-directory-symbolic-link
https://msdn.microsoft.com/de-de/library/windows/desktop/aa365006(v=vs.85).aspx

# Rechte vergeben

## icacls

`ICACLS name /save aclfile \[/T\] \[/C\] \[/L\] \[/Q\]`
Speichert die DACLs für die Dateien und Ordner mit übereinstimmendem Namen zur späteren Verwendung mit "/restore" in der ACL-Datei. SACLs, Besitzer oder Integritätsbezeichnungen werden nicht gespeichert.

`ICACLS Verzeichnis \[/substitute SidOld SidNew \[...\]\] /restore aclfile \[/C\] \[/L\] \[/Q\]`
Wendet die gespeicherten ACLs auf die Dateien im Verzeichnis an.

`ICACLS name /setowner user \[/T\] \[/C\] \[/L\] \[/Q\]`
Ändert den Besitzer für alle übereinstimmenden Namen. Diese Option erzwingt keine Änderung der Besitzrechte. Verwenden Sie dazu das Hilfsprogramm "takeown.exe".

`ICACLS name /findsid Sid \[/T\] \[/C\] \[/L\] \[/Q\]`
Findet alle übereinstimmenden Namen, die eine ACL enthalten, in der die SID explizit erwähnt wird.

`ICACLS name /verify \[/T\] \[/C\] \[/L\] \[/Q\]`
Findet alle Dateien, deren ACL kein kanonisches Format aufweist oder deren Länge nicht mit der ACE-Anzahl übereinstimmt.

`ICACLS name /reset \[/T\] \[/C\] \[/L\] \[/Q\]`
Ersetzt die ACLs für alle übereinstimmenden Dateien durch standardmäßige vererbte ACLs.

ICACLS name \[/grant\[:r\] Sid:perm\[...\]\] \[/deny Sid:perm \[...\]\] \[/remove\[:g|:d\]\] Sid\[...\]\] \[/T\] \[/C\] \[/L\] \[/Q\] \[/setintegritylevel Level:policy\[...\]\]

`/grant\[:r\] Sid:perm` gewährt die angegebenen Benutzerzugriffsrechte. Mit :r ersetzen die Berechtigungen alle zuvor gewährten expliziten Berechtigungen. Ohne :r, werden die Berechtigungen beliebigen zuvor gewährten expliziten Berechtigungen hinzugefügt.

`/deny Sid:perm` verweigert die angegebenen Benutzerzugriffsrechte explizit. Eine explizit verweigerte ACE wird für die angegebenen Berechtigungen hinzugefügt, und die gleichen Berechtigungen in einer expliziten Berechtigung werden entfernt.

`/remove\[:\[g|d\]\]` Sid entfernt alle Vorkommen gewährter Rechten für diese SID. Mit :g, werden alle Vorkommen von gewährten Rechten für diese entfernt. Mit :d, werden alle Vorkommen von verweigerten Rechten für diese SID entfernt.

`/setintegritylevel \[(CI)(OI)\]Level` fügt allen übereinstimmenden Dateien explizit eine Integritäts-ACE hinzu. Die Ebene wird mit einem der folgenden Werte angegeben: **L\[ow\] M\[edium\] H\[igh\]** Vererbungsoptionen für die Integritäts-ACE können vor der Ebene stehen und werden nur auf Verzeichnisse angewendet.

/inheritance:e|d|r e - aktiviert die Vererbung. d - deaktiviert die Vererbung und kopiert die ACEs. r - entfernt alle vererbten ACEs.

  
> Hinweis:
> IDs können im numerischen Format oder als Anzeigenamen angegeben werden.
> Fügen Sie beim numerischen Format ein \* am Anfang der SID hinzu.
{.is-info}


/T gibt an, dass dieser Vorgang für alle übereinstimmenden Dateien/Verzeichnisse ausgeführt wird, die den im Namen angegebenen Verzeichnissen untergeordnet sind.

/C gibt an, dass dieser Vorgang bei allen Dateifehlern fortgesetzt wird. Fehlermeldungen werden weiterhin angezeigt.

/L gibt an, dass dieser Vorgang für einen symbolischen Link anhand seines Ziels ausgeführt wird.

/Q gibt an, dass ICACLS Erfolgsmeldungen unterdrücken soll.

ICACLS behält die kanonische Sortierung der ACE-Einträge bei: Explizite Verweigerungen Explizite Berechtigungen Vererbte Verweigerungen Vererbte Berechtigungen

`perm` ist eine Berechtigungsmaske und kann in einem von zwei Formaten angegeben werden:
- Eine Folge von einfachen Rechten:
  - N - Kein Zugriff
  - F - Vollzugriff
  - M - Änderungszugriff
  - RX - Lese- und Ausführungszugriff
  - R - Schreibgeschützter Zugriff
  - W - Lesegeschützter Zugriff
  - D - Löschzugriff
- Eine in Klammern stehende kommagetrennte Liste von bestimmten Rechten: 
  - DE - Löschen
  - RC - Lesesteuerung
  - WDAC - DAC schreiben
  - WO - Besitzer schreiben
  - S - Synchronisieren
  - AS - Systemsicherheitszugriff
  - MA - Maximal zulässig
  - GR - Allgemeiner Lesezugriff
  - GW - Allgemeiner Schreibzugriff
  - GE - Allgemeiner Ausführungszugriff
  - GA - Allgemeiner Zugriff (alle)
  - RD - Daten lesen/Verzeichnis auflisten
  - WD - Daten schreiben/Datei hinzufügen
  - AD - Daten anfügen/Unterverzeichnis hinzufügen
  - REA - Erweiterte Attribute lesen
  - WEA - Erweiterte Attribute schreiben
  - X - Ausführen/Durchsuchen
  - DC - Untergeordnetes Element löschen
  - RA - Attribute lesen
  - WA - Attribute schreiben
- Die Vererbungsrechte können beiden Formaten vorangestellt werden und werden nur auf Verzeichnisse angewendet:
  - (OI) - Objektvererbung
  - (CI) - Containervererbung
  - (IO) - Nur vererben
  - (NP) - Vererbung nicht verteilen
  - (I) - Vom übergeordneten Container vererbte Berechtigung

Beispiele:

- `icacls "D:\test" /grant John:(OI)(CI)F`
Gewährt dem Benutzer John Vollzugriff auf den Ordner D:\Test und Vererbt die Berechtigung an alle Unterordner und Dateien.

- `icacls c:\windows\* /save AclFile /T`
Speichert die ACLs für alle Dateien unter "c:\windows" und in den dazugehörigen Unterverzeichnissen in der ACL.

- `icacls c:\windows\ /restore AclFile`
Stellt die ACLs für alle Dateien in der ACL-Datei wieder her, die unter "c:\windows" und in den dazugehörigen Unterverzeichnissen vorhanden sind.

- `icacls file /grant Administrator:(D,WDAC)`
Gewährt dem Benutzer mit Administratorrechten die Berechtigungen "DAC löschen" und "DAC schreiben" für die Datei.

- `icacls file /grant *S-1-1-0:(D,WDAC)`
Gewährt dem durch die SID S-1-1-0 definierten Benutzer die Berechtigungen "DAC löschen" und "DAC schreiben" für die Datei.


### <Quelle:

http://stackoverflow.com/questions/2928738/how-to-grant-permission-to-users-for-a-directory-using-command-line-in-windows

# dism

## DotNet 3.5 installieren

`DISM /Online /Enable-Feature /FeatureName:NetFx3 /All /LimitAccess /Source:d:\sources\sxs`

# Prozess als Dienst installieren

Hierzu werden Drittanbieter Tool benötigt, ein Favorit ist **"SRVSTART.EXE"**, zu finden im Quell Link.  
Als kurze Beschreibung wie man einfach eine EXE als Dienst hinterlegt geht man folgendermaßen vor.

1. Herunterladen und z.B. in C:\\SRVSTART entpacken (srvstart.exe sollte in unserem Beispiel in dem Ordner sein)
2. Überprüfen ob "msvcrt.dll" im System32 Ordner vorhanden ist (sollte es standardmäßig sein)
3. In dem gleichen Ordner eine "srvstart.ini" erstellen mit folgendem Inhalt
\[Dienstname_zusammengeschrieben\]
startup=C:\\Programmpfad\\Programm.EXE
4. CMD als Admin öffnen
5. Dienst installieren mit folgendem Befehl `srvstart.exe install Dienstname -c C:\SRVSTART\SRVSTART.INI`
6. Dienst starten mit `net start Dienstname`

## Quelle:
https://github.com/rozanski/srvstart
http://www.rozanski.org.uk/software (Originale Seite, verweist auf die Github Seite)

# xcopy

## Kopieren von Ordner und Unterordnern mit NTFS und Freigabeberechtigung

`Xcopy Quelle Ziel /O /X /E /H /K /Y /C`

## Beschreibung

- `/O` Kopiert Informationen über den Besitzer und die ACL.
- `/X` Kopiert Dateiüberwachungseinstellungen (bedingt /O).
- `/E` Kopiert alle Unterverzeichnisse (leer oder nicht leer). Wie /S /E. Mit dieser Option kann die Option /T geändert werden.
- `/H` Kopiert auch Dateien mit den Attributen "Versteckt" und "System".
- `/K` Kopiert Attribute. Standardmäßig wird "Schreibgeschützt" gelöscht.
- `/Y` Unterdrückt die Aufforderung zur Bestätigung, dass eine vorhandene Zieldatei überschrieben werden soll.
- `/C` Setzt das Kopieren fort, auch wenn Fehler auftreten.

## Quelle:

https://www.ubackup.com/backup-restore/xcopy-command-to-copy-folders-and-subfolders-6688.html

# wmic

## Produktschlüssel ( Key ) aus dem BIOS auslesen

`wmic path softwarelicensingservice get OA3xOriginalProductKey`