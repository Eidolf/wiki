---
title: offentliche-ordner-verwaltungstools
description: 
published: true
date: 2023-12-31T14:34:15.504Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:12.270Z
---

# Öffentliche Ordner Verwaltungstools

# <span class="mw-headline" id="bkmrk-pfdavadmin%3A">PFDAVADMIN:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=%C3%96ffentliche_Ordner_Verwaltungstools&action=edit&section=1 "Abschnitt bearbeiten: PFDAVADMIN:")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-info%3A">Info:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=%C3%96ffentliche_Ordner_Verwaltungstools&action=edit&section=2 "Abschnitt bearbeiten: Info:")<span class="mw-editsection-bracket">\]</span></span>

PFDAVADMIN wurde für Exchange 2003 und Exchange 2007 verwendet und bei Exchange 2010 durch FolderEX ersetzt

## <span class="mw-headline" id="bkmrk-installation%3A">Installation:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=%C3%96ffentliche_Ordner_Verwaltungstools&action=edit&section=3 "Abschnitt bearbeiten: Installation:")<span class="mw-editsection-bracket">\]</span></span>

Die Installation sollte auf einem Client erfolgen und nicht auf dem Exchange Server selbst

### <span class="mw-headline" id="bkmrk-downloadquelle%3A">Downloadquelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=%C3%96ffentliche_Ordner_Verwaltungstools&action=edit&section=4 "Abschnitt bearbeiten: Downloadquelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://www.microsoft.com/downloads/details.aspx?FamilyID=635BE792-D8AD-49E3-ADA4-E2422C0AB424&displaylang=en" rel="nofollow">http://www.microsoft.com/downloads/details.aspx?FamilyID=635BE792-D8AD-49E3-ADA4-E2422C0AB424&displaylang=en</a>
```

## <span class="mw-headline" id="bkmrk-installationsproblem-1">Installationsprobleme</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=%C3%96ffentliche_Ordner_Verwaltungstools&action=edit&section=5 "Abschnitt bearbeiten: Installationsprobleme")<span class="mw-editsection-bracket">\]</span></span>

Folgende Meldung kann nach der Installation auftreten, beim öffnen eines öffentlichen Ordners

```
'Could not expand https://localhost/exadmin/admin/mydomain.com/public%20folders/ :
name cannot begin with the '0' character, hexadecimal value 0x30. Line 1, position 386'
```

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=%C3%96ffentliche_Ordner_Verwaltungstools&action=edit&section=6 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Es fehlt DotNetFramework 1.1 auf dem ausführenden Client, nur sollte man es nicht auf einem Exchange Server 2007 installieren, da eventuell die Bibliotheken vorhandene überschreiben.

### <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=%C3%96ffentliche_Ordner_Verwaltungstools&action=edit&section=7 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://mostlyexchange.blogspot.de/2008/01/pfdavadmin-exchange-2007-and-v11-net.html" rel="nofollow">http://mostlyexchange.blogspot.de/2008/01/pfdavadmin-exchange-2007-and-v11-net.html</a>
```