---
title: dpm-update-agent-passwort-fur-arbeitsplatzrechner
description: 
published: true
date: 2023-12-31T14:32:24.026Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:32:20.817Z
---

# DPM Update Agent Passwort für Arbeitsplatzrechner

# <span class="mw-headline" id="bkmrk-fehlermeldung-1">Fehlermeldung</span>

Unable to contact DPM protection agent.  
The protection agent operation failed because it could not access the protection agent on  
YOURPROTECTEDDPMSERVER. YOURPROTECTEDDPMSERVER may be running DPM, or the DPM protection  
agent may have been installed by another DPM Server.

# <span class="mw-headline" id="bkmrk-eingabe-auf-client-1">Eingabe auf Client</span>

```
SetDpmServer.exe -dpmServerName YOURDPMSERVER -isNonDomainServer -updatePassword
```

# <span class="mw-headline" id="bkmrk-eingabe-auf-dpm-serv-1">Eingabe auf DPM Server</span>

```
Update-NonDomainServerInfo –PSName YOURPROTECTEDDPMSERVER –dpmServerName YOURDPMSERVER
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://edroszcz.wordpress.com/2011/01/15/update-dpm-agent-password-on-none-domain-joined-servers/" rel="nofollow">http://edroszcz.wordpress.com/2011/01/15/update-dpm-agent-password-on-none-domain-joined-servers/</a>
```