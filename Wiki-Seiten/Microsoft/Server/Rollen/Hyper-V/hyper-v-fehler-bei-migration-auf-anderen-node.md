---
title: hyper-v-fehler-bei-migration-auf-anderen-node
description: 
published: true
date: 2023-12-31T14:34:39.726Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:36.594Z
---

# Hyper-V Fehler bei Migration auf anderen Node

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-laufwerksabh%C3%A4ngigkei-1">Laufwerksabhängigkeit überprüfen</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Fehler_bei_Migration_auf_anderen_Node&action=edit&section=1 "Abschnitt bearbeiten: Laufwerksabhängigkeit überprüfen")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-den-failoverclusterm"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-den-failoverclusterm-1" lang="de"><div class="mw-parser-output">1. Den Failoverclustermanager öffnen <dl><dd>[Datei:HyperV Abhaengigkeit 03.jpg](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=HyperV_Abhaengigkeit_03.jpg "Datei:HyperV Abhaengigkeit 03.jpg")</dd></dl>
2. Unter Clustername --&gt; Dienste und Anwendungen --&gt; Virtueller Computername einen Rechtsklick auf 
    1. Virtueller Computer. <dl><dd>[Datei:HyperV Abhaengigkeit 02.jpg](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=HyperV_Abhaengigkeit_02.jpg "Datei:HyperV Abhaengigkeit 02.jpg")</dd><dd>Hier prüfen ob unter dem Reiter Abhängigkeit die Konfiguration des virtuellen Computers und das Laufwerk angegeben sind.</dd></dl>
    2. Konfiguration des virtuellen Computers. <dl><dd>[Datei:HyperV Abhaengigkeit 01.jpg](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=HyperV_Abhaengigkeit_01.jpg "Datei:HyperV Abhaengigkeit 01.jpg")</dd><dd>Hier prüfen ob unter dem Reiter Abhängigkeit das Laufwerk angegeben ist.</dd></dl>

</div></div></div>