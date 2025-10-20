---
title: dpm-fehlermeldungen
description: 
published: true
date: 2023-12-31T14:31:52.015Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:31:48.415Z
---

# DPM Fehlermeldungen

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-wsb-event-id%3A-521%2C-w-1">WSB Event ID: 521, WSB Error Code: 0x807800A1</span>

## <span class="mw-headline" id="bkmrk-fehlermeldung%3A-1">Fehlermeldung:</span>

```
DPM cannot create a backup because Windows Server Backup (WSB) on the protected computer encountered an error
(WSB Event ID: 521, WSB Error Code:  0x807800A1).
(ID 30229 Details: Internal error code: 0x809909FB)
```

## <span class="mw-headline" id="bkmrk-in-der-ereignisanzei-1">In der Ereignisanzeige ist dieser Fehler zu finden:</span>

```
Volumeschattenkopie-Dienstwarnung: ASR writer Error 0x80070001. hr = 0x00000000
Vorgang:

PrepareForBackup-Ereignis

PrepareForBackup-Ereignis

Kontext:

Ausführungskontext: ASR Writer

Ausführungskontext: Writer

Generatorklassen-ID: {be000cbe-11fe-4426-9c58-531aa6355fc4}

Generatorname: ASR Writer

Generatorinstanz-ID: {1ccde221-84af-4883-8f77-5d59953bca5e}
```

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=4 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Es ist scheinbar eine OEM Partition vorhanden und als aktive Partition geschalten, es muss die die Primäre Partition aktiv geschalten werden.

<div class="vector-body" id="bkmrk-diskpart-select-disk"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Diskpart
2. **select disk** (disk mit OEM/Recovery/C: Partition)
3. **select Partition** (disk mit OEM/Recovery die als Systempartition angezeigt wird)
4. **inactive**
5. **select Partition** (die Hauptsystempartion meist C:)
6. **active**
7. Diskpart mit Exit beenden
8. BCDBOOT c:\\windows /s C: eingeben (Wenn Systempartiton C: ist)
9. Neustart
10. Überprüfen ob OEM/Recovery Partiton nicht mehr als Systempartition angegeben ist

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=5 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://itcookbook.net/blog/removing-windows-7-recovery-partition" rel="nofollow">http://itcookbook.net/blog/removing-windows-7-recovery-partition</a>
<a class="external free" href="http://social.technet.microsoft.com/forums/en-US/winservergen/thread/f5e53544-3a00-4863-a5bf-594b3310c4f2/" rel="nofollow">http://social.technet.microsoft.com/forums/en-US/winservergen/thread/f5e53544-3a00-4863-a5bf-594b3310c4f2/</a>
```

# <span class="mw-headline" id="bkmrk-id%3A-3055-0x8000ffff">ID: 3055 0x8000FFFF</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=6 "Abschnitt bearbeiten: ID: 3055 0x8000FFFF")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-fehlermeldung%3A-2">Fehlermeldung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=7 "Abschnitt bearbeiten: Fehlermeldung:")<span class="mw-editsection-bracket">\]</span></span>

Wenn man versucht ein Item Level Recovery durchzuführen, erscheint bei dem Versuch auf die VHD Datei zuzugreifen folgende Meldung.

```
DPM is unable to enumerate on computer (dpmserver). Please ensure that ist accessible to protection agent.
ID: 3055
Details: Catastrophic Failure (0x8000FFFF)
```

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=8 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Überprüfen ob Automount am DPM Server aktiviert ist

<div class="vector-body" id="bkmrk-cmd-%C3%B6ffnen-diskpart%C2%A0"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. CMD öffnen
2. **diskpart** eingeben
3. **automount** eingeben

</div></div></div>Wenn es deaktiviert ist, folgende Befehle in eine Kommandokonsole eingeben

<div class="vector-body" id="bkmrk-cmd-%C3%B6ffnen-mountvol-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. CMD öffnen
2. **mountvol /e** eingeben

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=9 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-AU/dpmhypervbackup/thread/0e89671e-cc29-4643-b2c8-c60395f7dcd1" rel="nofollow">http://social.technet.microsoft.com/Forums/en-AU/dpmhypervbackup/thread/0e89671e-cc29-4643-b2c8-c60395f7dcd1</a>
```

# <span class="mw-headline" id="bkmrk-id%3A-30112-0x800423f3-1">ID: 30112 0x800423F3</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=10 "Abschnitt bearbeiten: ID: 30112 0x800423F3")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-fehlermeldung%3A-3">Fehlermeldung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=11 "Abschnitt bearbeiten: Fehlermeldung:")<span class="mw-editsection-bracket">\]</span></span>

Der DPM kann keine Virtuelle Gast Maschine auf einem bestimmten Cluster/Hyper-V Knoten sichern.

## <span id="bkmrk--3"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-2">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=12 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

**Hyper-V Virtual Machine Management** **(Hyper-V-Verwaltung für virtuelle Computer)** Dienst neu starten

## <span class="mw-headline" id="bkmrk-quelle%3A-2">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=13 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/winserverhyperv/thread/3b6e3816-f8d4-4cd5-be25-6727ea4f31a7/" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/winserverhyperv/thread/3b6e3816-f8d4-4cd5-be25-6727ea4f31a7/</a>
```

## <span class="mw-headline" id="bkmrk-fehlermeldung2%3A">Fehlermeldung2:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=14 "Abschnitt bearbeiten: Fehlermeldung2:")<span class="mw-editsection-bracket">\]</span></span>

Der DPM kann keine Virtuelle Gast Maschine auf einem bestimmten Cluster/Hyper-V Knoten sichern auf dem ein SQL Server läuft mit mehreren Instanzen.

## <span class="mw-headline" id="bkmrk-zusatz%3A">Zusatz:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=15 "Abschnitt bearbeiten: Zusatz:")<span class="mw-editsection-bracket">\]</span></span>

Als Gegentest kann man probieren ob sich die zweite SQL Instanz sichern lässt.

## <span id="bkmrk--4"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung2%3A">Lösung2:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=16 "Abschnitt bearbeiten: Lösung2:")<span class="mw-editsection-bracket">\]</span></span>

Der für den SQL VSS Provider nötige Sicherheitsbenutzer NT-Autorität\\System muss auf jeder Instanz die **Systemadmin** Rolle haben

# <span class="mw-headline" id="bkmrk-id%3A-30111-0x800423f4-1">ID: 30111 0x800423F4</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=17 "Abschnitt bearbeiten: ID: 30111 0x800423F4")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-fehlermeldung%3A-4">Fehlermeldung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=18 "Abschnitt bearbeiten: Fehlermeldung:")<span class="mw-editsection-bracket">\]</span></span>

Wenn man versucht eine Synchronisierung von geschützten virtuellen Computern zu starten erscheint folgende Meldung.

```
Der Status von VSS Writer für Anwendung oder des VSS-Anbieters ist ungültig. Der Status ist entweder
beim Ausführen des aktuellen Vorgangs ungültig geworden oder war schon vorher ungültig.(ID 30111
VssError:Für den Generator wurde ein nicht vorübergehender Fehler festgestellt.
Wenn der Sicherungsvorgang wiederholt wird,tritt der Fehler höchstwahrscheinlich erneut auf. (0x800423F4))
```

## <span id="bkmrk--5"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-3">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=19 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Überprüfen ob Automount an den einzelnen Clusterknoten aktiviert ist, wenn nicht dann an allen Clusterknoten aktivieren

<div class="vector-body" id="bkmrk-cmd-%C3%B6ffnen-diskpart%C2%A0-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. CMD öffnen
2. **diskpart** eingeben
3. **automount** eingeben

</div></div></div>Wenn es deaktiviert ist, folgende Befehle in eine Kommandokonsole eingeben

<div class="vector-body" id="bkmrk-cmd-%C3%B6ffnen-mountvol--1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. CMD öffnen
2. **mountvol /e** eingeben

</div></div></div>## <span class="mw-headline" id="bkmrk-zusatz%3A-1">Zusatz:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=20 "Abschnitt bearbeiten: Zusatz:")<span class="mw-editsection-bracket">\]</span></span>

Bei einem zweiten DPM Server kann die gleiche Fehlermeldung etwas anderes bedeuteten, der zweite Link unter den Quellen beschreibt hier das Problem und bietet einen Lösungsvorschlag

## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=21 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://blogs.technet.com/b/davguents_blog/archive/2011/02/03/dpm-2010-backup-error-0x800423f4.aspx" rel="nofollow">http://blogs.technet.com/b/davguents_blog/archive/2011/02/03/dpm-2010-backup-error-0x800423f4.aspx</a>
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/dpmdrbackup/thread/413207bd-f907-45bb-b26b-7bcd0f6ada3a/" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/dpmdrbackup/thread/413207bd-f907-45bb-b26b-7bcd0f6ada3a/</a>
```

# <span class="mw-headline" id="bkmrk-id%3A-30111-0x80042308-1">ID: 30111 0x80042308</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=22 "Abschnitt bearbeiten: ID: 30111 0x80042308")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-fehlermeldung%3A-5">Fehlermeldung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=23 "Abschnitt bearbeiten: Fehlermeldung:")<span class="mw-editsection-bracket">\]</span></span>

Wenn man versucht eine Synchronisierung von geschützten SQL Datenbanken zu starten erscheint folgende Meldung.

```
Der Status von VSS Writer für Anwendung oder des VSS-Anbieters ist ungültig. Der Status ist entweder
beim Ausführen des aktuellen Vorgangs ungültig geworden oder war schon vorher ungültig.(ID 30111: Details: VssError:
Das angegebene Objekt wurde nicht gefunden. (0x80042308))
```

[Datei:DPM-Fehler-80042308.png](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=DPM-Fehler-80042308.png "Datei:DPM-Fehler-80042308.png")

## <span class="mw-headline" id="bkmrk-in-der-ereignisanzei-3">In der Ereignisanzeige ist dieser Fehler zu finden:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=24 "Abschnitt bearbeiten: In der Ereignisanzeige ist dieser Fehler zu finden:")<span class="mw-editsection-bracket">\]</span></span>

```
Ereignis-ID: 24581: Sqllib-Fehler: Die Systemtabelle 'sys.databases' in der SQL Server-Instanz XXX ist leer.
```

[Datei:Ereignis-24581-Uebersicht.png](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=Ereignis-24581-Uebersicht.png "Datei:Ereignis-24581-Uebersicht.png")  
[Datei:Ereignis-24581.png](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=Ereignis-24581.png "Datei:Ereignis-24581.png")

**Meldung auf Microsoft Internetseite die Übereinstimmt:**

```
Meldung 3
Wenn Sie SQL Server VSS Writer verwenden, um eine Sicherungskopie erstellen, können Sie die folgende Fehlermeldung auftreten:

Protokollname: Anwendung
Quelle: SQLWRITER
Ereignis-ID: 24581
Aufgabe Kategorie: keine
Stufe: Fehler
Beschreibung:
Sqllib-Fehler: sys.sysdatabases der System-Tabelle in SQL Server-Instanz < SQL Server-Name > ist leer. 
Wenn das lokale Systemkonto ALTER ANY DATABASE oder VIEW ANY DATABASE Server-Level-Berechtigung oder CREATE DATABASE-Berechtigung in
der master-Datenbank gibt, wird dann über Fehler auftreten, da es nicht der Katalogsicht sys.Databases Abfragen kann.
Db_datareader Rechte ist nicht ausreichend für diesen Vorgang erfolgreich ausgeführt werden kann.
```

## <span id="bkmrk--6"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-4">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=25 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-sql-management-studi"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. SQL Management Studio öffnen
2. Auf Instanz verbinden die im Fehler angegeben ist
3. Eigenschaften der Instanz öffnen
4. Berechtigung auswählen
5. NT-AUTORITÄT\\SYSTEM hinzufügen
6. Berechtigung erteilen für **Ändern** und **Anzeigen** der Datenbank
7. Eigenschaften der **master** Datenbank öffnen
8. Berechtigung auswählen
9. NT-AUTORITÄT\\SYSTEM hinzufügen
10. Berechtigung erteilen für **Erstellen** der Datenbank

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-4">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Fehlermeldungen&action=edit&section=26 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://support.microsoft.com/kb/919023/de" rel="nofollow">http://support.microsoft.com/kb/919023/de</a>
```