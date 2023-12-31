# Kommandokonsole (CMD)

## <span class="mw-headline" id="bkmrk-bootrec-1">BootRec</span>

Problem mit dem Windows Boot Sektor bereinigen.

1. `bootrec /FixMBR`
2. `bootrec /ScanOS`
3. `bootrec /RebuildBCD`
4. `bootrec /FixBoot`

Falls kein Betriebssystem mehr zu finden ist kann man einen Booteintrag mit folgenden Befehl schreiben.

<dl id="bkmrk-bcdboot-c%3A%5Cwindows-%2F"><dd>`bcdboot C:\Windows /s C: /f BIOS`</dd></dl>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://appuals.com/how-to-fix-bootrec-fixboot-access-denied-on-windows-78-and-10/" rel="nofollow">https://appuals.com/how-to-fix-bootrec-fixboot-access-denied-on-windows-78-and-10/</a>
<a class="external free" href="https://www.computerwoche.de/a/windows-10-boot-manager-anpassen-entfernen-und-reparieren,2079024,2" rel="nofollow">https://www.computerwoche.de/a/windows-10-boot-manager-anpassen-entfernen-und-reparieren,2079024,2</a>
```

## <span class="mw-headline" id="bkmrk-scripting-1">Scripting</span>

[Batch erstellen / CMD Befehle](https://wiki.eidolf.de/index.php/Batch_erstellen_/_CMD_Befehle "Batch erstellen / CMD Befehle")

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-symbolischer-link-%28--1">Symbolischer Link ( mklink )</span>

1. mklinks werden dazu benötigt Dateien oder auch Pfade Umzuleiten, das sozusagen die Zieldatei die geändert wird von jedem beliebigen Startpunkt bearbeiten kann. Das erste Beispiel ist der Standard mit mklink ohne Option, nämlich eine Datei zu verknüpfen. <dl><dd>`mklink E:\Ausgangsordner\Ausgangsdatei.txt C:\Users\Zielordner\Zieldatei.txt`</dd></dl>
2. Mit **/j** erstellt man eine Ordnerweiterleitung. Dies wird primär für eine Umleitung eines Ordners zu einem anderen Pfad verwendet. Dringend nötig auch für Remotezugriff von anderen Rechnern da symbolische Links zu einem Pfad dies nicht können. Weiterhin darf Ausgangsordner nicht vorhanden sein, dieser wird durch den Befehl angelegt. <dl><dd>`mklink /j E:\Ausgangsordner C:\Users\Zielordner`</dd></dl>
3. Mit **/d** erstellt man einen symbolischen Link zu einem Pfad, der Standard (Punkt 1) ohne Option ist zu einer Datei, er ist auch ähnlich wie mit Option /j doch ist ein Remotezugriff problematisch. <dl><dd>`mklink /d E:\Ausgangsordner C:\Users\Zielordner`</dd></dl>
4. Mit **/h** erstellt man einen Hard link. Hardlinks verknüpfen Dateien in verschiedenen Ordnern aber auf dem gleichen Volume. <dl><dd>`mklink /h C:\Ausgangsordner\Ausgangsdatei.txt C:\Users\Zielordner\Zieldatei.txt`</dd></dl>

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://technet.microsoft.com/de-de/library/cc753194(v=ws.10).aspx" rel="nofollow">https://technet.microsoft.com/de-de/library/cc753194(v=ws.10).aspx</a>
<a class="external free" href="https://superuser.com/questions/343074/directory-junction-vs-directory-symbolic-link" rel="nofollow">https://superuser.com/questions/343074/directory-junction-vs-directory-symbolic-link</a>
<a class="external free" href="https://msdn.microsoft.com/de-de/library/windows/desktop/aa365006(v=vs.85).aspx" rel="nofollow">https://msdn.microsoft.com/de-de/library/windows/desktop/aa365006(v=vs.85).aspx</a>
```

## <span class="mw-headline" id="bkmrk-rechte-vergeben-1">Rechte vergeben</span>

### <span class="mw-headline" id="bkmrk-icacls-1">icacls</span>

ICACLS name /save aclfile \[/T\] \[/C\] \[/L\] \[/Q\] Speichert die DACLs für die Dateien und Ordner mit übereinstimmendem Namen zur späteren Verwendung mit "/restore" in der ACL-Datei. SACLs, Besitzer oder Integritätsbezeichnungen werden nicht gespeichert.

ICACLS Verzeichnis \[/substitute SidOld SidNew \[...\]\] /restore aclfile \[/C\] \[/L\] \[/Q\] Wendet die gespeicherten ACLs auf die Dateien im Verzeichnis an.

ICACLS name /setowner user \[/T\] \[/C\] \[/L\] \[/Q\] Ändert den Besitzer für alle übereinstimmenden Namen. Diese Option erzwingt keine Änderung der Besitzrechte. Verwenden Sie dazu das Hilfsprogramm "takeown.exe".

ICACLS name /findsid Sid \[/T\] \[/C\] \[/L\] \[/Q\] Findet alle übereinstimmenden Namen, die eine ACL enthalten, in der die SID explizit erwähnt wird.

ICACLS name /verify \[/T\] \[/C\] \[/L\] \[/Q\] Findet alle Dateien, deren ACL kein kanonisches Format aufweist oder deren Länge nicht mit der ACE-Anzahl übereinstimmt.

ICACLS name /reset \[/T\] \[/C\] \[/L\] \[/Q\] Ersetzt die ACLs für alle übereinstimmenden Dateien durch standardmäßige vererbte ACLs.

ICACLS name \[/grant\[:r\] Sid:perm\[...\]\] \[/deny Sid:perm \[...\]\] \[/remove\[:g|:d\]\] Sid\[...\]\] \[/T\] \[/C\] \[/L\] \[/Q\] \[/setintegritylevel Level:policy\[...\]\]

/grant\[:r\] Sid:perm gewährt die angegebenen Benutzerzugriffsrechte. Mit :r ersetzen die Berechtigungen alle zuvor gewährten expliziten Berechtigungen. Ohne :r, werden die Berechtigungen beliebigen zuvor gewährten expliziten Berechtigungen hinzugefügt.

/deny Sid:perm verweigert die angegebenen Benutzerzugriffsrechte explizit. Eine explizit verweigerte ACE wird für die angegebenen Berechtigungen hinzugefügt, und die gleichen Berechtigungen in einer expliziten Berechtigung werden entfernt.

/remove\[:\[g|d\]\] Sid entfernt alle Vorkommen gewährter Rechten für diese SID. Mit :g, werden alle Vorkommen von gewährten Rechten für diese entfernt. Mit :d, werden alle Vorkommen von verweigerten Rechten für diese SID entfernt.

/setintegritylevel \[(CI)(OI)\]Level fügt allen übereinstimmenden Dateien explizit eine Integritäts-ACE hinzu. Die Ebene wird mit einem der folgenden Werte angegeben: L\[ow\] M\[edium\] H\[igh\] Vererbungsoptionen für die Integritäts-ACE können vor der Ebene stehen und werden nur auf Verzeichnisse angewendet.

/inheritance:e|d|r e - aktiviert die Vererbung. d - deaktiviert die Vererbung und kopiert die ACEs. r - entfernt alle vererbten ACEs.

  
Hinweis:

<dl id="bkmrk-sids-k%C3%B6nnen-im-numer"><dt>SIDs können im numerischen Format oder als Anzeigenamen angegeben werden.</dt><dt>Fügen Sie beim numerischen Format ein \* am Anfang der SID hinzu.</dt></dl>/T gibt an, dass dieser Vorgang für alle übereinstimmenden Dateien/Verzeichnisse ausgeführt wird, die den im Namen angegebenen Verzeichnissen untergeordnet sind.

/C gibt an, dass dieser Vorgang bei allen Dateifehlern fortgesetzt wird. Fehlermeldungen werden weiterhin angezeigt.

/L gibt an, dass dieser Vorgang für einen symbolischen Link anhand seines Ziels ausgeführt wird.

/Q gibt an, dass ICACLS Erfolgsmeldungen unterdrücken soll.

ICACLS behält die kanonische Sortierung der ACE-Einträge bei: Explizite Verweigerungen Explizite Berechtigungen Vererbte Verweigerungen Vererbte Berechtigungen

perm ist eine Berechtigungsmaske und kann in einem von zwei Formaten angegeben werden: Eine Folge von einfachen Rechten: N - Kein Zugriff F - Vollzugriff M - Änderungszugriff RX - Lese- und Ausführungszugriff R - Schreibgeschützter Zugriff W - Lesegeschützter Zugriff D - Löschzugriff Eine in Klammern stehende kommagetrennte Liste von bestimmten Rechten: DE - Löschen RC - Lesesteuerung WDAC - DAC schreiben WO - Besitzer schreiben S - Synchronisieren AS - Systemsicherheitszugriff MA - Maximal zulässig GR - Allgemeiner Lesezugriff GW - Allgemeiner Schreibzugriff GE - Allgemeiner Ausführungszugriff GA - Allgemeiner Zugriff (alle) RD - Daten lesen/Verzeichnis auflisten WD - Daten schreiben/Datei hinzufügen AD - Daten anfügen/Unterverzeichnis hinzufügen REA - Erweiterte Attribute lesen WEA - Erweiterte Attribute schreiben X - Ausführen/Durchsuchen DC - Untergeordnetes Element löschen RA - Attribute lesen WA - Attribute schreiben Die Vererbungsrechte können beiden Formaten vorangestellt werden und werden nur auf Verzeichnisse angewendet: (OI) - Objektvererbung (CI) - Containervererbung (IO) - Nur vererben (NP) - Vererbung nicht verteilen (I) - Vom übergeordneten Container vererbte Berechtigung

Beispiele:

```
       icacls "D:\test" /grant John:(OI)(CI)F
       - Gewährt dem Benutzer John Vollzugriff auf den Ordner D:\Test
         und Vererbt die Berechtigung an alle Unterordner und Dateien.
```

```
       icacls c:\windows\* /save AclFile /T
       - Speichert die ACLs für alle Dateien unter "c:\windows"
         und in den dazugehörigen Unterverzeichnissen in der ACL.
```

```
       icacls c:\windows\ /restore AclFile
       - Stellt die ACLs für alle Dateien in der ACL-Datei wieder her,
         die unter "c:\windows" und in den dazugehörigen Unterverzeichnissen
         vorhanden sind.
```

```
       icacls file /grant Administrator:(D,WDAC)
       - Gewährt dem Benutzer mit Administratorrechten die Berechtigungen
         "DAC löschen" und "DAC schreiben" für die Datei.
```

```
       icacls file /grant *S-1-1-0:(D,WDAC)
       - Gewährt dem durch die SID S-1-1-0 definierten Benutzer die
         Berechtigungen "DAC löschen" und "DAC schreiben" für die Datei.
```

#### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="http://stackoverflow.com/questions/2928738/how-to-grant-permission-to-users-for-a-directory-using-command-line-in-windows" rel="nofollow">http://stackoverflow.com/questions/2928738/how-to-grant-permission-to-users-for-a-directory-using-command-line-in-windows</a>
```

## <span class="mw-headline" id="bkmrk-dism-1">dism</span>

### <span class="mw-headline" id="bkmrk-dotnet-3.5-installie-1">DotNet 3.5 installieren</span>

```
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All /LimitAccess /Source:d:\sources\sxs
```

## <span class="mw-headline" id="bkmrk-prozess-als-dienst-i-1">Prozess als Dienst installieren</span>

Hierzu werden Drittanbieter Tool benötigt, ein Favorit ist **"SRVSTART.EXE"**, zu finden im Quell Link.  
Als kurze Beschreibung wie man einfach eine EXE als Dienst hinterlegt geht man folgendermaßen vor.

1. Herunterladen und z.B. in C:\\SRVSTART entpacken (srvstart.exe sollte in unserem Beispiel in dem Ordner sein)
2. Überprüfen ob "msvcrt.dll" im System32 Ordner vorhanden ist (sollte es standardmäßig sein)
3. In dem gleichen Ordner eine "srvstart.ini" erstellen mit folgendem Inhalt <dl><dd>\[Dienstname\_zusammengeschrieben\]</dd><dd>startup=C:\\Programmpfad\\Programm.EXE</dd></dl>
4. CMD als Admin öffnen
5. Dienst installieren mit folgendem Befehl `srvstart.exe install Dienstname -c C:\SRVSTART\SRVSTART.INI`
6. Dienst starten mit `net start Dienstname`

### <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="http://www.rozanski.org.uk/software" rel="nofollow">http://www.rozanski.org.uk/software</a>
<a class="external free" href="http://www.rozanski.org.uk/services_quick" rel="nofollow">http://www.rozanski.org.uk/services_quick</a>
```

## <span class="mw-headline" id="bkmrk-xcopy-1">xcopy</span>

### <span class="mw-headline" id="bkmrk-kopieren-von-ordner--1">Kopieren von Ordner und Unterordnern mit NTFS und Freigabeberechtigung</span>

`Xcopy Quelle Ziel /O /X /E /H /K /Y /C`

### <span class="mw-headline" id="bkmrk-beschreibung-1">Beschreibung</span>

- `/O` Kopiert Informationen über den Besitzer und die ACL.
- `/X` Kopiert Dateiüberwachungseinstellungen (bedingt /O).
- `/E` Kopiert alle Unterverzeichnisse (leer oder nicht leer). Wie /S /E. Mit dieser Option kann die Option /T geändert werden.
- `/H` Kopiert auch Dateien mit den Attributen "Versteckt" und "System".
- `/K` Kopiert Attribute. Standardmäßig wird "Schreibgeschützt" gelöscht.
- `/Y` Unterdrückt die Aufforderung zur Bestätigung, dass eine vorhandene Zieldatei überschrieben werden soll.
- `/C` Setzt das Kopieren fort, auch wenn Fehler auftreten.

### <span class="mw-headline" id="bkmrk-quelle%3A-9">Quelle:</span>

```
<a class="external free" href="https://www.ubackup.com/backup-restore/xcopy-command-to-copy-folders-and-subfolders-6688.html" rel="nofollow">https://www.ubackup.com/backup-restore/xcopy-command-to-copy-folders-and-subfolders-6688.html</a>
```

## <span class="mw-headline" id="bkmrk-wmic-1">wmic</span>

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-produktschl%C3%BCssel-%28-k-1">Produktschlüssel ( Key ) aus dem BIOS auslesen</span>

`wmic path softwarelicensingservice get OA3xOriginalProductKey`