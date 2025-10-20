---
title: scom-cscriptexe-nologo
description: 
published: true
date: 2023-12-31T13:35:18.496Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:35:15.406Z
---

# SCOM Cscript.exe //nologo

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-scom-agent-unter-dem-1">SCOM Agent unter dem Lokalen Systemkonto auf einem Domänen Kontroller ausführen</span>

After deploying the OpsMgr agent to a DC you may receive some errors concerning scripts not being able to run. An example of an error you may receive is below:  
Forced to terminate the following process started at 1:01:51 PM because it ran past the configured timeout 120 seconds.  
Fehler:

```
Command executed: “C:\Windows\system32\cscript.exe” //nologo 
“C:\Program Files\System Center Operations Manager 2007\Health Service State\Monitoring Host 
Temporary Files 5\3201\AD_Replication_Partner_Op_Master_Consistency.vbs” DC01.domain.pvt false
Working Directory: C:\Program Files\System Center Operations Manager 2007\
Health Service State\Monitoring Host Temporary Files 5\3201\
```

  
This is do to the fact that the SCOM HealthService be default only allows for “NT AUTHORITY\\Authenticated Users” Users to access the service. However the service is running as LocalSystem by default. LocalSystem doesn’t fall under the Authenticated Users scope. This is where the hslockdown tool comes in.  
  
CMD öffnen und folgenden Befehl eingeben

```
“C:\Program Files\System Center Operations Manager 2007\hslockdown.exe” “SITENAME” /L
```

hier erkennt man das nur der "Authenticated Users" Eingetragen und erlaubt ist auf den Dienst zuzugreifen.

You will need to add LocalSystem to the allowed list for the HealthService and restart the HealthService for the changes to take effect. Issue the following commands in the command prompt to complete the desired task:

```
"C:\Program Files\System Center Operations Manager 2007\hslockdown.exe" "SITENAME" /A "NT AUTHORITY\SYSTEM"
sc stop HealthService
sc start HealthService
```

Quelle:

```
<a class="external free" href="http://www.blogmynog.com/2009/11/04/running-scom-agent-on-domain-controller-as-local-system/#more-182" rel="nofollow">http://www.blogmynog.com/2009/11/04/running-scom-agent-on-domain-controller-as-local-system/#more-182</a>
```