---
title: band-formatierungs-probleme
description: 
published: true
date: 2023-12-31T13:32:22.537Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:32:19.447Z
---

# Band Formatierungs Probleme

Falls Bänder vom DPM nicht gelöscht werden können, muss man es mit einem Commandshell Tool von MS löschen. Der Programmname ist "mytape1.exe"

<div class="vector-body" id="bkmrk-mount-media-to-be-er"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Mount media to be erased into drive.
2. Open command prompt and type below command
3. &gt;mytape1.exe \\\\.\\tape0 {è where \\\\.\\tape0 is logical path name of drive in which media is mounted)
4. Then use erasetape -&gt; s short erase option as shown belo in snapshot.
5. Once it finishes use 'q' option to exit.

</div></div></div>  
DPM konforme Bänder erstellen (Skript+mytape.exe)  
[Datei:DPMeraseTape.zip](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=DPMeraseTape.zip "Datei:DPMeraseTape.zip")

Quelle:

```
<a class="external free" href="http://www.eggheadcafe.com/software/aspnet/32001361/115-so-far-to-erase-a-ta.aspx" rel="nofollow">http://www.eggheadcafe.com/software/aspnet/32001361/115-so-far-to-erase-a-ta.aspx</a>
<a class="external free" href="http://blogs.technet.com/b/dpm/archive/2010/07/18/erasing-unsupported-tapes-amp-utility-automation-sample.aspx" rel="nofollow">http://blogs.technet.com/b/dpm/archive/2010/07/18/erasing-unsupported-tapes-amp-utility-automation-sample.aspx</a>
```