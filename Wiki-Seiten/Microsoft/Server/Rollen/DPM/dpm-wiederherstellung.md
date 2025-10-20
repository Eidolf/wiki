---
title: dpm-wiederherstellung
description: 
published: true
date: 2023-12-31T14:32:38.582Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:32:35.455Z
---

# DPM Wiederherstellung

# <span class="mw-headline" id="bkmrk-vorgehen%3A-1">Vorgehen:</span>

Nachfolgendes Vorgehen hat die besten Ergebnisse wenn der Zielserver am ende den gleichen Namen bekommt wie der Quellserver. Bei Unterschiedlichen Namen muss manuell den Client Agents, der neue Zielserver als Zieladresse angegeben werden.

# <span class="mw-headline" id="bkmrk-dpm-2010%3A-1">DPM 2010:</span>

## <span class="mw-headline" id="bkmrk-auf-quellserver%3A-1">Auf Quellserver:</span>

<div class="vector-body" id="bkmrk-dpmbackup--db-%28savin"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. dpmbackup -db (saving current database)
2. uninstall DPM 2010 with volume retention
3. delete DPMDB database from SQL Server 2008 (in SQL Management Studio)

</div></div></div>## <span class="mw-headline" id="bkmrk-auf-zielserver%3A-1">Auf Zielserver:</span>

<div class="vector-body" id="bkmrk-install-dpm-2010-res"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. install DPM 2010
2. rescan an add DPM disks (with old volumes)
3. rescan and add library
4. attach agents for existing production servers (via script or DPM 2010 interface)
5. dpmsync restoredb -dbloc d:\\dpmdb.bak
6. dpmsync -sync (from command prompt in ..\\dpm\\bin directory)

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Wiederherstellung&action=edit&section=5 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://msgroups.net/microsoft.public.dataprotectionmanager/disaster-recovery-of-d/59632" rel="nofollow">http://msgroups.net/microsoft.public.dataprotectionmanager/disaster-recovery-of-d/59632</a>
<a class="external free" href="http://technet.microsoft.com/en-us/library/ff399388.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/ff399388.aspx</a>
```