---
title: dpm-installation
description: 
published: true
date: 2023-12-31T14:32:14.226Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:32:09.501Z
---

# DPM Installation

# <span class="mw-headline" id="bkmrk-dpm-2012">DPM 2012</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=1 "Abschnitt bearbeiten: DPM 2012")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-voraussetzungen">Voraussetzungen</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=2 "Abschnitt bearbeiten: Voraussetzungen")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-dom%C3%A4ne-beigetreten-i"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Domäne beigetreten
2. Installation DotNetFramework 3.5 
    - Unter Server 2012 Folgend:
    
    
    1. Server 2012 DVD eingelegt
    2. PowerShell öffnen und folgenden Befehl eingeben
    3. Install-WindowsFeature -name Net-Framework-Core -source D:\\sources\\sxs\\
3. Installation SIS Filter 
    - Befehl ausführen C:\\&gt; dism /online /enable-feature:SIS-Limited

</div></div></div>## <span class="mw-headline" id="bkmrk-installation">Installation</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=3 "Abschnitt bearbeiten: Installation")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-dpm-setup-ausf%C3%BChren-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. DPM Setup ausführen
2. SQL Lokal auswählen und Installieren (hier kann auch eine bestehende Instanz auf einem SQL Server gewählt werden) 
    - Bei DPM 2012 R2 wird eine richtig SQL Instanz vorausgesetzt mit folgenden Einstellungen
    
    
    1. Features: 
        1. Database Engine Services
        2. Full-Text and Semantic Extractions for Search
        3. Reporting Services - Native
        4. Management Tools
    2. Einstellungen: 
        1. Zur Einfachheit eine Default Instanz
        2. **SQL Server Agent** auf **Automatic** setzen
        3. Einen Domänenservice Benutzer angeben für folgende Dienste **Agent**, **Engine**, und **Reporting**.
        4. Collation auf **SQL\_Latin1\_General\_CP1\_CI\_AS** stellen
        5. Admin Verwaltungsgruppe angeben für die SQL Verwaltung
3. Fertigstellen

</div></div></div># <span class="mw-headline" id="bkmrk-dpm-2010-beta-auf-dp-1">DPM 2010 Beta auf DPM 2010 RC</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=4 "Abschnitt bearbeiten: DPM 2010 Beta auf DPM 2010 RC")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-probleme">Probleme</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=5 "Abschnitt bearbeiten: Probleme")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-wenn-am-dpm-server-2"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Wenn am DPM Server 2010 eine Evaluierungsversion installiert war und danach die RC Version nachinstalliert wurde, kann es sein das der SQL Server sich nicht mehr öffnen lässt mit der Fehlermeldung "Die Evaluierung ist abgelaufen..."

</div></div></div>Unter dem SQL Configuration Manager sind beide Instanzen zu finden und auch nach dem deaktivieren der alten Instanz treten in der Ereignisanzeige weiterhin Fehlermeldungen auf.

<div class="vector-body" id="bkmrk-in-der-dienste-verwa"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- In der Dienste Verwaltung kann man weiterhin die SQL und SQL Reporting Dienste nicht starten oder stoppen.

</div></div></div>  
[![DPMBeta-RC 01.png](https://wiki.eidolf.de/images/8/87/DPMBeta-RC_01.png)](https://wiki.eidolf.de/index.php/Datei:DPMBeta-RC_01.png)

<div class="vector-body" id="bkmrk-die-%C3%9Cberpr%C3%BCfung-ergi"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Die Überprüfung ergibt das noch die EVAL Version als Zweitinstanz aktiv ist von der vorherigen BETA Version des DPM 2010.

- Diese muss man jetzt über "Programm entfernen" in der Systemsteuerung löschen. Es wird der SQL Install Wizard aufgerufen bei dem man einfach über "Remove" die Instanz löscht.

</div></div></div>  
"Remove" auswählen [![DPMBeta-RC 02.png](https://wiki.eidolf.de/images/d/dc/DPMBeta-RC_02.png)](https://wiki.eidolf.de/index.php/Datei:DPMBeta-RC_02.png)

<div class="vector-body" id="bkmrk-die-veraltete%2Fabgela"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Die Veraltete/Abgelaufende Instanz auswählen

</div></div></div>  
[![DPMBeta-RC 03.png](https://wiki.eidolf.de/images/6/64/DPMBeta-RC_03.png)](https://wiki.eidolf.de/index.php/Datei:DPMBeta-RC_03.png)

  
Und danach einfach den Wizard durchklicken

# <span class="mw-headline" id="bkmrk-manuelle-installatio-1">Manuelle Installation des Agents</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=6 "Abschnitt bearbeiten: Manuelle Installation des Agents")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-on-the-computer-on-w"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. On the computer on which you want to install the protection agent, we recommend that you map a network drive to the DPM server. For example, from the command prompt, type ```
    net use Z: \\DPM1\c$.
    ```
2. On the protected computer, from the command prompt, change the directory (CD) to z:\\Program Files\\Microsoft DPM\\DPM\\Agents\\RA\\2.0.8861.0\\i386 (if you have a 64-bit computer, replace \\i386 with AM64), and then type DpmAgentInstaller.exe &lt;DPM server name&gt;. ```
    For example: DPMAgentInstaller.exe DPM1.Fully.qualified.domain
    ```
    
    OR  
    On a 64-bit computer, type DPMAgentInstaller\_amd64.exe &lt;DPM server name&gt;.  
    Note If you use the DPM server name in the command line, DPM installs the protection agent and configures the security permissions for the DPM server.  
      
    You can perform a non-interactive installation by specifying a /q parameter after the DpmAgentInstaller.exe command.  
    For example, type
    
    ```
    DpmAgentInstaller.exe /q <DPM server name>.
    ```
3. Restart the protected server.  
    Note The following step is not required if you specified the DPM server in Step 1.
4. To complete the protection agent configuration for the appropriate DPM server and firewall settings, from the command prompt, type &lt;drive letter&gt;:\\Program Files\\Microsoft Data Protection Manager\\DPM\\bin\\SetDpmServer.exe – dpmServerName &lt;DPM server name&gt;.  
    For example: ```
    SetDpmServer.exe –dpmServerName DPM01
    ```
    
    Where DPM01 is the actual DPM server name.
5. On the DPM server, from the DPM Management Shell prompt, type Attach-ProductionServer.ps1 &lt;DPM server name&gt; &lt;production server name&gt; &lt;user name&gt; &lt;password&gt; &lt;domain&gt;.  
    The password parameter is not required and we recommend that you do not provide it. DPM will prompt you for a password, which will not appear on the screen. However, you can provide the password if you want to use the script to install a protection agent on a large number of servers.

</div></div></div>  
Quelle:

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/bb870935.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/bb870935.aspx</a>
```

## <span class="mw-headline" id="bkmrk-manuelle-installatio-3">Manuelle Installation des Agents auf einem Arbeitsgruppenrechner</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=7 "Abschnitt bearbeiten: Manuelle Installation des Agents auf einem Arbeitsgruppenrechner")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-hinweis%3Af%C3%BCr-jeden-ar"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- **Hinweis**:Für jeden Arbeitsgruppenrechner wird ein eigener Benutzer benötigt, der von der Client Seite aus erstellt und danach am DPM Server bekannt gemacht wird.

</div></div></div>  
Die Installation ist bis Punkt 4 gleich, bei Punkt 4 wird der Befehl in folgenden geändert.

```
SetDpmServer.exe –dpmServerName DPM01 -isNonDomainServer -UserName dpmagent
```

Dadurch wird ein lokaler Benutzer angelegt mit dem vergebenen Passwort, dieser wird für den Agent benötigt.

Danach kann man auf dem DPM Server mit dem gerade angelegten Benutzer entweder über die GUI den Server hinzufügen , oder über folgenden Befehl in der DPM Shell

```
Attach-NonDomainServer.ps1 <DPM server name> <production server name> <user name> <password>
```

# <span id="bkmrk--4"></span><span class="mw-headline" id="bkmrk-firewalleinstellunge-1">Firewalleinstellungen für DPM</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=DPM_Installation&action=edit&section=8 "Abschnitt bearbeiten: Firewalleinstellungen für DPM")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-protocol-ports-detai"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output"><table cellspacing="5" width="100%"><tbody><tr><td width="20%">**Protocol**</td><td width="20%">**Ports**</td><td width="60%">**Details**</td></tr><tr><td width="20%">DCOM</td><td width="20%">135/TCP  
Dynamic</td><td width="60%">The DPM control protocol uses DCOM. DPM issues commands to the file agent by invoking DCOM calls on the agent. The file agent responds by invoking DCOM calls on the DPM server.  
TCP port 135 is the DCE endpoint resolution point used by DCOM  
By default, DCOM assigns ports dynamically from the TCP port range of 1024 through 65535. You can, however, configure this range by using Component Services. For more information, see Using Distributed COM with Firewalls ([http://go.microsoft.com/fwlink/?LinkId=46088](http://go.microsoft.com/fwlink/?LinkId=46088)).</td></tr><tr><td>TCP</td><td>3148/TCP  
3149/TCP</td><td>The DPM data channel is based on TCP. Both DPM and the file server initiate connections to enable DPM operations such as synchronization and recovery. DPM communicates with the agent coordinator on port 3148 and with the file agent on port 3149.</td></tr><tr><td>DNS</td><td>53/UDP</td><td>Used between DPM and the domain controller, and between the file server and the domain controller, for host name resolution.</td></tr><tr><td>Kerberos</td><td>88/UDP  
88/TCP</td><td>Used between DPM and the domain controller, and between the file server and the domain controller, for authentication of the connection endpoint.</td></tr><tr><td>LDAP</td><td>389/TCP  
389/UDP</td><td>Used between DPM and the domain controller for Active Directory queries.</td></tr><tr><td>NetBIOS</td><td>137/UDP  
138/UDP  
139/TCP</td><td>Used between DPM and the file server, between DPM and the domain controller, and between the file server and the domain controller, for miscellaneous operations.</td></tr></tbody></table>

</div></div></div>  
Quelle:

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/cc161275.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/cc161275.aspx</a>
```