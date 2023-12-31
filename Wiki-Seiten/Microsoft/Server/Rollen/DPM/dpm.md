# DPM

# <span class="mw-headline" id="bkmrk-dpm-allgemein-1">DPM Allgemein</span>

## <span class="mw-headline" id="bkmrk-festplatten-migriere-1">Festplatten Migrieren</span>

<div class="vector-body" id="bkmrk-%24disk-%3D-get-dpmdisk-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. $disk = Get-DPMDisk -DPMServerName RKW2K3-DPM
2. ./MigrateDatasourceDataFromDPM.ps1 -DPMServerName &lt;DPM Server Name&gt; -Source $disk\[n\] -Destination $disk\[n\]

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://blogs.technet.com/b/askcore/archive/2009/06/22/how-to-use-the-migratedatasourcedatafromdpm-ps1-dpm-powershell-script-to-move-data.aspx" rel="nofollow">http://blogs.technet.com/b/askcore/archive/2009/06/22/how-to-use-the-migratedatasourcedatafromdpm-ps1-dpm-powershell-script-to-move-data.aspx</a>
```

## <span class="mw-headline" id="bkmrk-versionsnummern-1">Versionsnummern</span>

[TechNet Link](http://social.technet.microsoft.com/wiki/contents/articles/4058.list-of-build-numbers-for-system-center-data-protection-manager-dpm.aspx%7C)

## <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

### <span class="mw-headline" id="bkmrk-ein-anderer-dpm-serv-1">Ein anderer DPM-Server ist der Besitzer</span>

#### <span class="mw-headline" id="bkmrk-angezeigte-fehler%3A-1">Angezeigte Fehler:</span>

```
Der Sicherungs- bzw. Wiederherstellungsauftrag für die Datenquelle konnte nicht ausgeführt werden, weil ein anderer DPM-Server der Besitzer ist.
Datenquelle: Servername\MSDPMINSTANCE\ReportServer$MSDPMINSTANCE
DPM-Besitzerserver:  (ID 3184 Details: Die Datei oder das Verzeichnis ist beschädigt und nicht lesbar (0x80070570))
```

#### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

Unter dem Pfad  
`<Installationsplatte>:\Programme\Microsoft Data Protection Manager\dpm\activeowner`  
oder bei Azure Backup unter  
`<Installationsplatte>:\Programme\Microsoft Azure Backup\DPM\DPM\ActiveOwner`  
Alle 0KB Dateien umbenennen in z.B. XXXX.old  
Sicherung kann wieder gestartet werden.  
Für weiter Fehlerbehebungen unter dem Quell Link nachschauen

#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://support.risualblogs.com/blog/2015/04/13/dpm-2012-r2-replica-inconsistent-data-source-owned-by-a-different-dpm-server/" rel="nofollow">http://support.risualblogs.com/blog/2015/04/13/dpm-2012-r2-replica-inconsistent-data-source-owned-by-a-different-dpm-server/</a>
```

# <span class="mw-headline" id="bkmrk-dpm-upgrade-1">DPM Upgrade</span>

## <span class="mw-headline" id="bkmrk-dpm-2012-r2-zu-dpm-2-1">DPM 2012 R2 zu DPM 2016</span>

<div class="vector-body" id="bkmrk-durchpatchen-zu-dpm-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Durchpatchen zu DPM Version 2012 R2 **Update Rollup 10**
2. Falls SQL 2008 R2 auch durchpatchen auf die aktuellste Version
3. Installer entpacken danach starten  
    [![SCDPM2016-01.png](https://wiki.eidolf.de/images/thumb/e/e2/SCDPM2016-01.png/700px-SCDPM2016-01.png)](https://wiki.eidolf.de/index.php/Datei:SCDPM2016-01.png)
4. Lizenzabkommen bestätigen  
    [![SCDPM2016-02.png](https://wiki.eidolf.de/images/0/03/SCDPM2016-02.png)](https://wiki.eidolf.de/index.php/Datei:SCDPM2016-02.png)
5. Warnung bestätigen  
    [![SCDPM2016-03.png](https://wiki.eidolf.de/images/thumb/5/5b/SCDPM2016-03.png/700px-SCDPM2016-03.png)](https://wiki.eidolf.de/index.php/Datei:SCDPM2016-03.png)
6. Lokale SQL Instanz prüfen und danach Installation starten  
    [![SCDPM2016-04.png](https://wiki.eidolf.de/images/thumb/b/b5/SCDPM2016-04.png/700px-SCDPM2016-04.png)](https://wiki.eidolf.de/index.php/Datei:SCDPM2016-04.png)
7. Nach Installation einiger Grundkomponenten kommen folgende Hinweise  
    [![Install-DPM2016-03.png](https://wiki.eidolf.de/images/thumb/f/f3/Install-DPM2016-03.png/700px-Install-DPM2016-03.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-03.png)
8. Neustart und Setup nochmal durchführen
9. Windows Update aufrufen und DPM 2016 Update Rollup 1 installieren
10. Wenn das Update durchgeführt wurde müssen die Agents auf den aktuellen Stand gebracht werden.
11. Zusatzinfo: Eine nicht supportete Lösung ist danach ein InPlace Upgrade des Servers von 2012-R2 auf 2016. Bei mir hat es funktioniert und ich konnte sogar nach einigem Umstand die neue Speicherart verwenden. (Zu empfehlen ist diese Lösung aber nicht für eine Produktivumgebung)

</div></div></div>## <span class="mw-headline" id="bkmrk-dpm-2012-zu-dpm-2012-1">DPM 2012 zu DPM 2012 R2</span>

### <span class="mw-headline" id="bkmrk-fehler-bei-sql-updat-1">Fehler bei SQL Update</span>

Wenn beim Internen SQL Update des DPM Install Wizards ein Fehler kommt der aussagt das kein richtiger SQL Server installiert ist und weiterhin bei öffnen des **SQL Server Configuration Manager** ein WMI Fehler auftritt kann es folgendermaßen gelöst werden.

<div class="vector-body" id="bkmrk-administrative-conso"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Administrative Console öffnen
2. mofcomp.exe “C:\\Program Files (x86)\\Microsoft SQL Server\\100\\Shared\\sqlmgmproviderxpsp2up.mof”
3. Überprüfen ob die Instanz auf TCP/IP hört und aktiviert ist

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="http://edsitonline.com/2013/11/05/upgrade-dpm-2012-sp1-to-dpm-2012-r2/comment-page-1/#comment-240" rel="nofollow">http://edsitonline.com/2013/11/05/upgrade-dpm-2012-sp1-to-dpm-2012-r2/comment-page-1/#comment-240</a>
```

## <span class="mw-headline" id="bkmrk-dpm-2010-beta-auf-dp-1">DPM 2010 Beta auf DPM 2010 RC</span>

### <span class="mw-headline" id="bkmrk-probleme-1">Probleme</span>

<div class="vector-body" id="bkmrk-wenn-am-dpm-server-2"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Wenn am DPM Server 2010 eine Evaluierungsversion installiert war und danach die RC Version nachinstalliert wurde, kann es sein das der SQL Server sich nicht mehr öffnen lässt mit der Fehlermeldung "Die Evaluierung ist abgelaufen..."

</div></div></div>Unter dem SQL Configuration Manager sind beide Instanzen zu finden und auch nach dem deaktivieren der alten Instanz treten in der Ereignisanzeige weiterhin Fehlermeldungen auf.

<div class="vector-body" id="bkmrk-in-der-dienste-verwa"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- In der Dienste Verwaltung kann man weiterhin die SQL und SQL Reporting Dienste nicht starten oder stoppen.

</div></div></div>[![DPMBeta-RC 01.png](https://wiki.eidolf.de/images/8/87/DPMBeta-RC_01.png)](https://wiki.eidolf.de/index.php/Datei:DPMBeta-RC_01.png)

<div class="vector-body" id="bkmrk-die-%C3%9Cberpr%C3%BCfung-ergi"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Die Überprüfung ergibt das noch die EVAL Version als Zweitinstanz aktiv ist von der vorherigen BETA Version des DPM 2010.
- Diese muss man jetzt über "Programm entfernen" in der Systemsteuerung löschen. Es wird der SQL Install Wizard aufgerufen bei dem man einfach über "Remove" die Instanz löscht.

</div></div></div>"Remove" auswählen [![DPMBeta-RC 02.png](https://wiki.eidolf.de/images/d/dc/DPMBeta-RC_02.png)](https://wiki.eidolf.de/index.php/Datei:DPMBeta-RC_02.png)

<div class="vector-body" id="bkmrk-die-veraltete%2Fabgela"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Die Veraltete/Abgelaufende Instanz auswählen

</div></div></div>[![DPMBeta-RC 03.png](https://wiki.eidolf.de/images/6/64/DPMBeta-RC_03.png)](https://wiki.eidolf.de/index.php/Datei:DPMBeta-RC_03.png)  
Und danach einfach den Wizard durchklicken

# <span class="mw-headline" id="bkmrk-dpm-installation-1">DPM Installation</span>

## <span class="mw-headline" id="bkmrk-dpm-2016-1">DPM 2016</span>

### <span class="mw-headline" id="bkmrk-voraussetzungen-1">Voraussetzungen</span>

#### <span class="mw-headline" id="bkmrk-sql-server-1">SQL Server</span>

<div class="vector-body" id="bkmrk-dotnet-3.5-installie"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. [DotNet 3.5 installieren](https://wiki.eidolf.de/index.php/Shell#DotNet_3.5_installieren "Shell")
2. Starten von SQL 2014 EN Standard Installer (Englisch verwende ich zur Sicherheit das weniger Fehler auftreten)  
    [![Install-DPM2016-SQL2014-01.png](https://wiki.eidolf.de/images/thumb/0/08/Install-DPM2016-SQL2014-01.png/700px-Install-DPM2016-SQL2014-01.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-01.png)
3. Produktkey und Lizenzabkommen bestätigen
4. Microsoft Update abfragen ist optional
5. Voraussetzungen Prüfen (Windows Firewall Warnung sagt nur aus, das Sie aktiviert ist und man alle Ports richtig öffnen soll)  
    [![Install-DPM2016-SQL2014-02.png](https://wiki.eidolf.de/images/thumb/a/a4/Install-DPM2016-SQL2014-02.png/700px-Install-DPM2016-SQL2014-02.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-02.png)
6. Feature basierte Installation auswählen, da nicht alle Rollen benötigt werden  
    [![Install-DPM2016-SQL2014-03.png](https://wiki.eidolf.de/images/thumb/c/c0/Install-DPM2016-SQL2014-03.png/700px-Install-DPM2016-SQL2014-03.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-03.png)
7. Folgende Features auswählen  
    [![Install-DPM2016-SQL2014-04.png](https://wiki.eidolf.de/images/thumb/d/da/Install-DPM2016-SQL2014-04.png/700px-Install-DPM2016-SQL2014-04.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-04.png)  
    [![Install-DPM2016-SQL2014-05.png](https://wiki.eidolf.de/images/thumb/6/61/Install-DPM2016-SQL2014-05.png/300px-Install-DPM2016-SQL2014-05.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-05.png)
8. Named Instance auwählen und betiteln  
    [![Install-DPM2016-SQL2014-06.png](https://wiki.eidolf.de/images/thumb/b/be/Install-DPM2016-SQL2014-06.png/700px-Install-DPM2016-SQL2014-06.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-06.png)
9. Jeweils einen [Managed Service Account](https://wiki.eidolf.de/index.php/Managed_Service_Accounts "Managed Service Accounts") pro Service angeben. Die SQL Installationskonsole muss eventuell neu gestartet werden.  
    [![Install-DPM2016-SQL2014-07.png](https://wiki.eidolf.de/images/thumb/0/0d/Install-DPM2016-SQL2014-07.png/700px-Install-DPM2016-SQL2014-07.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-07.png)
10. Auf der gleichen Seite aber unter dem Reiter "Collation" den Zeichensatz ändern auf  
    [![Install-DPM2016-SQL2014-08.png](https://wiki.eidolf.de/images/thumb/8/84/Install-DPM2016-SQL2014-08.png/700px-Install-DPM2016-SQL2014-08.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-08.png)
11. Ich benutze die Windows Authentifizierung und füge Admin Benutzer hinzu  
    [![Install-DPM2016-SQL2014-09.png](https://wiki.eidolf.de/images/thumb/f/f2/Install-DPM2016-SQL2014-09.png/700px-Install-DPM2016-SQL2014-09.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-09.png)
12. Reportingservice soll gleich konfiguriert werden  
    [![Install-DPM2016-SQL2014-10.png](https://wiki.eidolf.de/images/thumb/5/59/Install-DPM2016-SQL2014-10.png/700px-Install-DPM2016-SQL2014-10.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-SQL2014-10.png)
13. Übersicht bestätigen

</div></div></div>### <span class="mw-headline" id="bkmrk-hauptinstallation-1">Hauptinstallation</span>

Wie man unter untenstehenden Punkt 5 sieht wird durch den Installer die HyperVPowershell benötigt, dies kann auch davor erledigt werden.  
`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Management-PowerShell`  
Bei meiner Installation habe ich den Neustart durchgeführt nach dem Hinweis. Alle weiteren [Voraussetzungen](https://technet.microsoft.com/en-us/system-center-docs/dpm/get-started/get-dpm-installed#BKMK_Prereq) werden jedenfalls ohne Probleme durch den Installer durchgeführt.

<div class="vector-body" id="bkmrk-installer-entpacken-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Installer entpacken danach starten  
    [![SCDPM2016-01.png](https://wiki.eidolf.de/images/thumb/e/e2/SCDPM2016-01.png/700px-SCDPM2016-01.png)](https://wiki.eidolf.de/index.php/Datei:SCDPM2016-01.png)
2. Lizenzabkommen bestätigen  
    [![SCDPM2016-02.png](https://wiki.eidolf.de/images/0/03/SCDPM2016-02.png)](https://wiki.eidolf.de/index.php/Datei:SCDPM2016-02.png)
3. Willkommensbildschirm überspringen  
    [![Install-DPM2016-01.png](https://wiki.eidolf.de/images/thumb/e/e5/Install-DPM2016-01.png/700px-Install-DPM2016-01.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-01.png)
4. SQL Instanz eintragen und auf **Prüfen und installieren** klicken  
    [![Install-DPM2016-02.png](https://wiki.eidolf.de/images/thumb/d/d6/Install-DPM2016-02.png/700px-Install-DPM2016-02.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-02.png)
5. Nach Installation einiger Grundkomponenten kommen folgende Hinweise  
    [![Install-DPM2016-03.png](https://wiki.eidolf.de/images/thumb/f/f3/Install-DPM2016-03.png/700px-Install-DPM2016-03.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-03.png)
6. Neustart durchgeführt und einen zweiten Durchgang gestartet
7. Geforderte Daten und Key eingeben  
    [![Install-DPM2016-04.png](https://wiki.eidolf.de/images/thumb/8/8f/Install-DPM2016-04.png/700px-Install-DPM2016-04.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-04.png)
8. Installationspfad auswählen  
    [![Install-DPM2016-05.png](https://wiki.eidolf.de/images/thumb/3/3b/Install-DPM2016-05.png/700px-Install-DPM2016-05.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-05.png)
9. Microsoft Update Einstellung auswählen
10. Übersicht bestätigen und auf **Installieren** klicken
11. Abschlussbildschirm  
    [![Install-DPM2016-06.png](https://wiki.eidolf.de/images/thumb/1/1e/Install-DPM2016-06.png/700px-Install-DPM2016-06.png)](https://wiki.eidolf.de/index.php/Datei:Install-DPM2016-06.png)
12. Nach der Installation sofort nochmal ein Windows Update durchführen um das Updaterollup 1 für SCDPM zu bekommen.

</div></div></div>## <span class="mw-headline" id="bkmrk-dpm-2012-1">DPM 2012</span>

### <span class="mw-headline" id="bkmrk-voraussetzungen-3">Voraussetzungen</span>

<div class="vector-body" id="bkmrk-dom%C3%A4ne-beigetreten-i"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Domäne beigetreten
2. Installation DotNetFramework 3.5 
    - Unter Server 2012 Folgend:
    
    
    1. Server 2012 DVD eingelegt
    2. PowerShell öffnen und folgenden Befehl eingeben
    3. Install-WindowsFeature -name Net-Framework-Core -source D:\\sources\\sxs\\
3. Installation SIS Filter 
    - Befehl ausführen C:\\&gt; dism /online /enable-feature:SIS-Limited

</div></div></div>### <span class="mw-headline" id="bkmrk-installation-1">Installation</span>

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

</div></div></div># <span class="mw-headline" id="bkmrk-dpm-einstellungen-1">DPM Einstellungen</span>

## <span class="mw-headline" id="bkmrk-dpm-agent-1">DPM Agent</span>

<div class="vector-body" id="bkmrk-falls-ehemals-gesich"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Falls ehemals gesicherte Clients noch den Agent installiert haben und man den DPM Server von Grund auf neu aufgebaut hat, der Clientagent deinstalliert werden um die neue Version zu installieren.

</div></div></div>### <span class="mw-headline" id="bkmrk-manuelle-installatio-1">Manuelle Installation des Agents</span>

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

#### <span class="mw-headline" id="bkmrk-manuelle-installatio-3">Manuelle Installation des Agents auf einem Arbeitsgruppenrechner</span>

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

## <span class="mw-headline" id="bkmrk-dpm-bare-metal-recov-1">DPM Bare Metal Recovery</span>

### <span class="mw-headline" id="bkmrk-test-welche-laufwerk-1">Test welche Laufwerke gesichert werden</span>

Da der DPM selbst nicht angibt welche Systemrelevanten Laufwerke unter der Bare Metal Sicherung zu finden sind muss man dies über das lokale wbadmin überprüfen.  
Also direkt auf den Zielrechner verbinden, eine CMD öffnen und folgenden Befehl eingeben

```
wbadmin start backup -backuptarget:\\Backupserver\backupfolder -allcritical
```

Diesen Befehl ausführen aber zum Schluß nicht mit Ja bestätigen, zeigt alle Laufwerke an die gesichert werden sollen bei allcritical (Bare Metal)

### <span class="mw-headline" id="bkmrk-probleme-daten-zu-si-1">Probleme Daten zu sichern</span>

Leider kann dies mehrere Ursachen haben, falls aber alles im DPM richtig aussieht und auf der Zielserverseite folgende Punkte zutreffen

<div class="vector-body" id="bkmrk-windows-server-backu"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Windows Server Backup ist installiert
- Server 2012 / 2012 R2
- Patchstand ist aktuell

</div></div></div>kann es eigentlich nur noch um eine falsche Speicherzuweisung für den Shadow Copy Dienst handeln.

<div class="vector-body" id="bkmrk-die-sicherung-unter-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Die Sicherung unter WSB bricht sofort ab <dl><dd>Hier liegt es höchstwahrscheinlich am Recovery Laufwerk</dd><dd>Lösung: Das Laufwerk zum Sichern des Shadow Copy Dienstes für das Recovery Laufwerk auf C: abändern und auf 1024MB erhöhen</dd></dl>
2. Die Sicherung bricht bei den ersten 95MB ab <dl><dd>Hier muss eine Änderung am C: Laufwerk selbst durchgeführt werden</dd><dd>Lösung: Beim Shadow Copy Dienst das Laufwerk C auf z.B. 10GB setzen unter Maximum Size stellen <dl><dd>Weiterhin kann es sein die DPM Schutzgruppe neu anzulegen und hier die Speichernutzung zu erhöhen.</dd></dl></dd></dl>

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle-1">Quelle</span>

```
<a class="external free" href="https://social.technet.microsoft.com/Forums/windows/en-US/7373a7b8-01c8-4e2b-aaaa-513b7dad56f4/windows-server-2012-r2-vm-back-up-fails-with-insufficient-storage-available-to-create-either-the?forum=windowsbackup" rel="nofollow">https://social.technet.microsoft.com/Forums/windows/en-US/7373a7b8-01c8-4e2b-aaaa-513b7dad56f4/windows-server-2012-r2-vm-back-up-fails-with-insufficient-storage-available-to-create-either-the?forum=windowsbackup</a>
```

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-firewalleinstellunge-1">Firewalleinstellungen für DPM</span>

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

## <span class="mw-headline" id="bkmrk-speicher-verwalten-1">Speicher verwalten</span>

### <span class="mw-headline" id="bkmrk-dpm-2016-speicher-1">DPM 2016 Speicher</span>

#### <span class="mw-headline" id="bkmrk-quelle%3A-9">Quelle:</span>

```
<a class="external free" href="https://technet.microsoft.com/en-us/system-center-docs/dpm/get-started/add-storage" rel="nofollow">https://technet.microsoft.com/en-us/system-center-docs/dpm/get-started/add-storage</a>
```

## <span class="mw-headline" id="bkmrk-bandlaufwerke-1">Bandlaufwerke</span>

[Bänder freischalten](https://wiki.eidolf.de/index.php/B%C3%A4nder_freischalten "Bänder freischalten")  
[Band Formatierungs Probleme](https://wiki.eidolf.de/index.php/Band_Formatierungs_Probleme "Band Formatierungs Probleme")