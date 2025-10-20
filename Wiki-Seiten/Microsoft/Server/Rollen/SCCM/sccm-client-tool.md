---
title: sccm-client-tool
description: 
published: true
date: 2023-12-31T13:34:50.284Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:34:46.923Z
---

# SCCM Client Tool

# <span class="mw-headline" id="bkmrk-beschreibung%3A-1">Beschreibung:</span>

SCCM Client Tool for

<div class="vector-body" id="bkmrk-fixing-wmi-issues-pi"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. fixing WMI issues
2. Ping issues
3. Client Health issues
4. change client cache size
5. Install / Uninstall SCCM Client
6. Repair Update Cycles .... etc

</div></div></div>The tool is downloadable as a ZIP file that contains four files:

```
ClientActionsTool.hta – The tool itself. 
Cmdkey.exe – command line tool for managing cached credentials. This is needed for alternate credentials feature when running the HTA on Windows XP. Cmdkey.exe is natively available starting from Windows Vista.
Wol.exe - command line tool from Gammadyne Corporation to send wake up packets to remote computers. 
Config.ini – A configuration file for default settings. This file is not required to run the tool, but if it doesn’t exist, the tool may prompt for the data every time it’s run.
```

## <span class="mw-headline" id="bkmrk-downloadlink%3A-1">Downloadlink:</span>

[SCCMTools.zip](http://gallery.technet.microsoft.com/SCCM-Client-Utility-WMI-ec270313/file/74814/1/SCCMTools.zip)

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://gallery.technet.microsoft.com/SCCM-Client-Utility-WMI-ec270313" rel="nofollow">http://gallery.technet.microsoft.com/SCCM-Client-Utility-WMI-ec270313</a>
```