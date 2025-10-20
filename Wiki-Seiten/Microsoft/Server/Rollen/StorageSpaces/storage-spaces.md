---
title: storage-spaces
description: 
published: true
date: 2023-12-31T14:36:13.606Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:36:10.355Z
---

# Storage Spaces

# <span class="mw-headline" id="bkmrk-anlegen-von-virtual--1">Anlegen von Virtual Disk</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Storage_Spaces&action=edit&section=1 "Abschnitt bearbeiten: Anlegen von Virtual Disk")<span class="mw-editsection-bracket">\]</span></span>

Probleme mit der Geschwindigkeit  
Für 4 Disk Parity verbund  
`New-VirtualDisk -FriendlyName "Your Disk Name" -StoragePoolFriendlyName "Your Pool Name" -NumberOfColumns 4 -ProvisioningType Thin -ResiliencySettingName Parity -Interleave 64KB -WriteCacheSize 0GB -size 5TB`

## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Storage_Spaces&action=edit&section=2 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://www.windowsnetworking.com/articles-tutorials/windows-server-2012/windows-server-2012-r2-storage-spaces-part1.html" rel="nofollow">http://www.windowsnetworking.com/articles-tutorials/windows-server-2012/windows-server-2012-r2-storage-spaces-part1.html</a>
<a class="external free" href="http://www.windowsnetworking.com/articles-tutorials/windows-server-2012/windows-server-2012-r2-storage-spaces-part2.html" rel="nofollow">http://www.windowsnetworking.com/articles-tutorials/windows-server-2012/windows-server-2012-r2-storage-spaces-part2.html</a>
<a class="external free" href="https://social.technet.microsoft.com/Forums/windowsserver/en-US/64aff15f-2e34-40c6-a873-2e0da5a355d2/parity-storage-space-so-slow-that-its-unusable?forum=winserver8gen" rel="nofollow">https://social.technet.microsoft.com/Forums/windowsserver/en-US/64aff15f-2e34-40c6-a873-2e0da5a355d2/parity-storage-space-so-slow-that-its-unusable?forum=winserver8gen</a>
```

# <span class="mw-headline" id="bkmrk-fehler">Fehler</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Storage_Spaces&action=edit&section=3 "Abschnitt bearbeiten: Fehler")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-speicher-wird-nicht--1">Speicher wird nicht richtig erkannt</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Storage_Spaces&action=edit&section=4 "Abschnitt bearbeiten: Speicher wird nicht richtig erkannt")<span class="mw-editsection-bracket">\]</span></span>

Problem ist das ein erweiterter Speicher in der Windows Datenträgerverwaltung richtig angezeigt wird aber Anwendungen den neu zugewiesenen Speicher nicht erkennen und als Fehlermeldung anzeigen das nicht genug freier Speicher zur Verfügung steht. Als zweites Indiz kann man auch ein wiederholtes erweitern des Storage Spaces nicht starten und dies bricht mit einer Fehlermeldung im Server Manager ab.

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Storage_Spaces&action=edit&section=5 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Mit folgenden Befehl der nur in PowerShell existiert und nicht in der GUI kann man den Storage Space optimieren um die Werte für Windows selbst in Ordnung zu ziehen. Eventuell können noch mehr Probleme dadurch behoben werden, aber in meinem speziellen Fall hat es geholfen das die Veeam Backupsoftware ihren Datenspeicher vollständig erkannt hat.  
Vorgang starten:  
`Get-StoragePool "pool-name" | Optimize-StoragePool`  
Vorgang überprüfen:  
`Get-StorageJob`

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Storage_Spaces&action=edit&section=6 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="https://social.technet.microsoft.com/Forums/en-US/ebe2d0f3-27ac-4e7d-92e9-37fe4feaab52/cant-resize-storage-spaces-virtual-disk?forum=winserverfiles" rel="nofollow">https://social.technet.microsoft.com/Forums/en-US/ebe2d0f3-27ac-4e7d-92e9-37fe4feaab52/cant-resize-storage-spaces-virtual-disk?forum=winserverfiles</a>
```