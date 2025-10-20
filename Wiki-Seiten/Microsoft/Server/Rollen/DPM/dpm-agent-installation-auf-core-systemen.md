---
title: dpm-agent-installation-auf-core-systemen
description: 
published: true
date: 2023-12-31T14:31:22.156Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:31:18.802Z
---

# DPM Agent Installation auf Core Systemen

# <span class="mw-headline" id="bkmrk-beschreibung%3A-1">Beschreibung:</span>

Der Agent lässt sich Standardmäßig nicht auf Windows Core Server installieren, über die Managementkonsole schlägt es fehl und bei einer manuelle Installation kann keine Verbindung zwischen dem Core Server und dem DPM aufgebaut werden.

# <span class="mw-headline" id="bkmrk-fehlermeldung%3A-1">Fehlermeldung:</span>

Bei dem versuch den Agent über die Management Konsole des DPM auszurollen kann folgende Fehlermeldung erscheinen.

```
Data Protection Manager Error ID: 270 
The agent operation failed on <protected server FQDN> because DPM could not communicate with the DPM
protection agent. The computer may be protected by another DPM server, or the protection agent may
have been uninstalled on the protected computer.
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

## <span class="mw-headline" id="bkmrk-configure-group-memb-1">Configure group memberships.</span>

There are the three groups we need to check. The DPM Server \*must\* be a member of the following groups. •Distributed COM Users •DPMRADCOMTrustedMachines •DPMRADmTrustedMachines

## <span class="mw-headline" id="bkmrk-do-a-manual-install--1">Do a manual install of the agent on core server.</span>

Follow the DPM 2010 steps from TechNet with the following changes:

  
a. Do use the most recent version of the RA from the DPM server

  
DPM 2007 +qfe go to \\Program Files\\Microsoft DPM\\DPM\\Agents\\RA\\2.0.xxxx.0\\AMD or i386. DPM 2010 +qfe go to \\Program Files\\Microsoft DPM\\DPM\\agents\\RA\\3.0.xxxx.0\\AMD or i386. DPM 2012 RTM go to \\Program Files\\Microsoft System Center 2012\\DPM\\DPM\\ProtectionAgents\\RA...

  
b. Do not worry about passing the DPM server name in during the install.

c. Do not reboot at the finish of the install if prompted.

## <span class="mw-headline" id="bkmrk-run-setdpmserver.exe-1">Run setdpmserver.exe on protected core server using the following command:</span>

setdpmserver -dpmservername &lt;DPM server netBIOS name&gt;

NOTE The executable is located in C:\\Program Files\\Microsoft Data Protection Manager\\DPM\\bin\\

If you get errors running the above command ignore them for now.

## <span class="mw-headline" id="bkmrk-reboot-the-windows-2-1">Reboot the Windows 2008 R2 core server.</span>

## <span class="mw-headline" id="bkmrk-run-attach-productio-1">Run Attach-ProductionServer on the DPM Server</span>

In the DPM Management Shell, run Attach-ProductionServer.ps1 as follows:

  
Attach-ProductionServer.ps1 &lt;DPM server name&gt; &lt;production server name&gt; &lt;user name&gt; &lt;password&gt; &lt;domain&gt;

Once the above steps are completed you may receive the errors in the Symptoms section above.

To configure the DCOM permissions you can build DCOMPERM from the SDK sample or you can download the executable from here.

Typically DCOMCNFG run from a remote server against the Windows Server Core server was the method to manage the Core server’s DCOM settings, however in Windows Server 2008 R2, DCOMCNFG.exe is not able to connect remotely to manage these.

Once you have obtained DCOMPERM.exe the following steps are used to find the application ID for the DPM RA service, view the existing permissions, and edit the settings as needed.

## <span class="mw-headline" id="bkmrk-list-the-dcom-applic-1">List the DCOM application ID for the DPM RA service:</span>

wmic dcomapp |findstr /i dpm

```
           {2DF31D97-33CC-4966-8FF9-F47C90F7D0F3}  DPM RA Service  DPM RA Service  DPM RA Service
```

## <span class="mw-headline" id="bkmrk-view-applicaiton-acc-1">View applicaiton access permissions:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Agent_Installation_auf_Core_Systemen&action=edit&section=10 "Abschnitt bearbeiten: View applicaiton access permissions:")<span class="mw-editsection-bracket">\]</span></span>

dcomperm -aa {2DF31D97-33CC-4966-8FF9-F47C90F7D0F3} list

```
           Access permission list for AppID {2DF31D97-33CC-4966-8FF9-F47C90F7D0F3}: 
           <Using Default Permissions>
```

## <span class="mw-headline" id="bkmrk-view-application-lau-1">View application launch permissions:</span>

dcomperm -al {2DF31D97-33CC-4966-8FF9-F47C90F7D0F3} list

```
           Launch permission list for AppID {2DF31D97-33CC-4966-8FF9-F47C90F7D0F3}: 
           <Using Default Permissions>
```

## <span class="mw-headline" id="bkmrk-set-application-laun-1">Set application launch permissions for DPMRA app:</span>

dcomperm -al {2DF31D97-33CC-4966-8FF9-F47C90F7D0F3} set fourthcoffee\\slight-dpm01$ permit level:ll,rl,la,ra

```
           Successfully set the Application Launch ACL. 
           Remote and Local launch permitted to NT AUTHORITY\SYSTEM. 
           Remote and Local activation permitted to NT AUTHORITY\SYSTEM. 
           Remote and Local launch permitted to BUILTIN\Administrators. 
           Remote and Local activation permitted to BUILTIN\Administrators. 
           Remote and Local launch permitted to NT AUTHORITY\INTERACTIVE. 
           Remote and Local activation permitted to NT AUTHORITY\INTERACTIVE. 
           Remote and Local launch permitted to FOURTHCOFFEE\SLIGHT-DPM01$. 
           Remote and Local activation permitted to FOURTHCOFFEE\SLIGHT-DPM01$.
```

Now see if agent communications is working correctly, if not, perform these additional steps. •Copy the C:\\Program Files\\Microsoft DPM\\DPM\\Setup\\SetAgentCfg.exe utility on the DPM Server to the Protected server.

## <span class="mw-headline" id="bkmrk-run-the-following-co-1">Run the following command</span>

SetAgentCfg.exe a DPMRA &lt;DPMservername&gt; DPMRADCOMTrustedMachines DPMRADmTrustedMachines

# <span class="mw-headline" id="bkmrk-zusatz-dpm-2012-1">Zusatz DPM 2012</span>

Beim DPM 2012 muss bevor das DPM Setup (setupdpm.exe) gestartet wird, erst VCREDIST installiert werden, dieses ist unter dem **DPM Installationspfad/ProtectionAgents/AC** zu finden

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://blogs.technet.com/b/dpm/archive/2012/05/22/how-to-install-the-dpm-agent-on-a-windows-server-2008-r2-core-computer.aspx" rel="nofollow">http://blogs.technet.com/b/dpm/archive/2012/05/22/how-to-install-the-dpm-agent-on-a-windows-server-2008-r2-core-computer.aspx</a>
```