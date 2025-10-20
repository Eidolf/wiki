---
title: dpm-gleichzeitig-verschiedene-daten-auf-ein-band
description: 
published: true
date: 2023-12-31T14:32:01.694Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:31:58.311Z
---

# DPM Gleichzeitig Verschiedene Daten auf ein Band

Aktivieren von Gleichzeitigen Daten

```
Set-DPMGlobalProperty -DpmServer <DPM Server Name> -OptimizeTapeUsage $true
```

Deaktivieren von Gleichzeitigen Daten

```
Set-DPMGlobalProperty -DpmServer <DPM Server Name> -OptimizeTapeUsage $false
```

  
Quelle:

```
<a class="external free" href="http://blogs.technet.com/b/dpm/archive/2008/07/06/enabling-disabling-co-location-of-data-on-tape.aspx" rel="nofollow">http://blogs.technet.com/b/dpm/archive/2008/07/06/enabling-disabling-co-location-of-data-on-tape.aspx</a>
<a class="external free" href="http://www.systemcenterblog.eu/?p=297" rel="nofollow">http://www.systemcenterblog.eu/?p=297</a>
```