---
title: win7-schreibschutz-auf-festplatte-partition
description: 
published: true
date: 2023-12-31T14:29:35.910Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:29:32.747Z
---

# Win7 Schreibschutz auf Festplatte / Partition

# <span class="mw-headline" id="bkmrk-info-1">Info</span>

Ein neues "Feature", was unter Windows 7 und somit auch unter Windows 2008 R2 eingeführt wurde, ist dass Festplatte, einzelne Partitionen, USB Sticks und sonstige Medien unter bestimmten Voraussetzungen nur schreibgeschützt eingebunden werden. So besteht dann im Windows-Explorer erst gar nicht die Möglichkeit neue Ordner oder Dateien anzulegen.

# <span class="mw-headline" id="bkmrk-schreibschutz-aufheb-1">Schreibschutz aufheben</span>

<div class="vector-body" id="bkmrk-diskpart-eingeben-fe"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. diskpart eingeben
2. Festplatte auswählen
3. ATTRIBUTES DISK CLEAR READONLY als Befehl eingeben

</div></div></div># <span class="mw-headline" id="bkmrk-quelle-1">Quelle</span>

```
<a class="external free" href="http://www.espend.de/artikel/schreibgeschuetzte-festplatte-partition-unter-windows-7-und-server-2008-r2.html" rel="nofollow">http://www.espend.de/artikel/schreibgeschuetzte-festplatte-partition-unter-windows-7-und-server-2008-r2.html</a>
```

```
<a class="external free" href="http://www.microsoft.com/windowsserver2003/evaluation/whyupgrade/supportedpaths.mspx" rel="nofollow">http://www.microsoft.com/windowsserver2003/evaluation/whyupgrade/supportedpaths.mspx</a>
```