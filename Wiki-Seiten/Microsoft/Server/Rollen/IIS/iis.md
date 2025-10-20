---
title: iis
description: 
published: true
date: 2023-12-31T11:55:34.385Z
tags: 
editor: markdown
dateCreated: 2023-12-31T11:55:31.466Z
---

# IIS

# <span class="mw-headline" id="bkmrk-remotezugriff-einric-1">Remotezugriff einrichten</span>

<div class="vector-body" id="bkmrk-voraussetzung-ist-da"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Voraussetzung ist das der Webserver schon eingerichtet ist
2. `Install-WindowsFeature Web-Mgmt-Service`
3. `Set-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\WebManagement\Server -Name EnableRemoteManagement -Value 1`
4. Webmanagement Dienst neu starten 
    1. Net Stop WMSVC
    2. Net Start WMSVC
5. Evtl. Firewallausnahme erstellen

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.sherweb.com/blog/manage-and-install-iis8-on-windows-2012-server-core/" rel="nofollow">http://www.sherweb.com/blog/manage-and-install-iis8-on-windows-2012-server-core/</a>
```