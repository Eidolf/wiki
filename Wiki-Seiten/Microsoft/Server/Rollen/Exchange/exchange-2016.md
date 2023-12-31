# Exchange 2016

## <span class="mw-headline" id="bkmrk-installation-1">Installation</span>

### <span class="mw-headline" id="bkmrk-sprachpakete-1">Sprachpakete</span>

Erste ein kurzer Hinweis wie man deinstalliert, wird bei Upgrades auf ein neues CU benötigt.

1. Exchange Installationsmedium einbinden
2. Zu eingebunden Medium in der Exchange Powershell wechseln
3. Deinstallation starten mit folgendem Befehl `.\setup.exe /removeumlanguagepack de-DE`

**Installation:**

1. Mit der Exchange Power Shell in den Pfad des Exchange Installationsmediums wechseln
2. Befehl ausführen <dl><dd>`Setup.com /AddUmLanguagePack:de-DE /s: H:\PfadzumUMPaket /IAcceptExchangeServerLicenseTerms`</dd></dl>

### <span class="mw-headline" id="bkmrk-server-1">Server</span>

1. DotNet 4.8 installieren
2. UCMA Runtime installieren
3. vcredist\_x64 von 2012 und 2013 installieren
4. Rollen mit Administrativer PowerShell installieren  
    `Install-WindowsFeature NET-WCF-HTTP-Activation45, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS`
5. Antivierenscanner für den Installationsvorgang kurzzeitig beenden.
6. Exchange Setup starten.

## <span class="mw-headline" id="bkmrk-installation-hinweis-1">Installation Hinweise</span>

### <span class="mw-headline" id="bkmrk-exchange-2016-in-201-1">Exchange 2016 in 2016 AD</span>

Falls ein Exchange 2016 in einem 2016 AD installiert werden soll wird mindestens ein Installationsmedium **EX 2016 CU3** benötigt.

#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://social.technet.microsoft.com/Forums/azure/de-DE/abeb1b39-6d2d-4faf-9c24-36d3d45f9dc2/installing-exchange-2016-on-server-2016-error-with-gui?forum=Exch2016SD" rel="nofollow">https://social.technet.microsoft.com/Forums/azure/de-DE/abeb1b39-6d2d-4faf-9c24-36d3d45f9dc2/installing-exchange-2016-on-server-2016-error-with-gui?forum=Exch2016SD</a>
```

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-sicherheit-erh%C3%B6hen-1">Sicherheit erhöhen</span>

Persönliche Meinung: Ist bisher mit Vorsicht zu genießen, da Funktionen wie z.B. bei der S4B Integration nicht mehr funktionieren, wie z.B. Sprachverlauf am Smartphone.

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://www.frankysweb.de/exchange-2016-installation-absichern-hardening/" rel="nofollow">https://www.frankysweb.de/exchange-2016-installation-absichern-hardening/</a>
```

## <span class="mw-headline" id="bkmrk-update-1">Update</span>

Exchange CU Updates sind eigentlich ein komplettes neu installieren des Exchange Server's und in meiner Umgebung mit folgenden Punkten am Besten durchzuführen.

1. Deinstallieren des UM Sprachpakets siehe &gt; [Exchange 2016 Sprachpakete](https://wiki.eidolf.de/index.php/Exchange#Sprachpakete "Exchange")
2. Exchange Installationsmedium an einem DC einbinden. 
    1. PowerShell öffnen und folgende Befehle als Domain Admin / Organisations Admin durchführen 
        1. `E:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareSchema`
        2. `E:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareAD /OrganizationName:"<Organization name>"`
            1. OrganizationName ist zu finden über PowerShell am Exchange mit folgendem Befehl `Get-OrganizationConfig | select Name`
        3. `E:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareAllDomains`
3. Installation des CU am Exchange Server
4. Installation Sprachpaket 
    1. Exchange Management Shell öffnen (Neuer Befehl ab CU21)
    2. `Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /AddUmLanguagePack:de-DE /SourceDir:"c:\install"`

## <span class="mw-headline" id="bkmrk-hybrid-1">Hybrid</span>

[Exchange 2016 Hybrid Konfiguration](https://wiki.eidolf.de/index.php/Exchange_2016_Hybrid_Konfiguration "Exchange 2016 Hybrid Konfiguration")  
Informationen zu Exchange Hybridszenarien in Verbindung mit O365

## <span class="mw-headline" id="bkmrk-fehler%3A-1">Fehler:</span>

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-exchange-2016-4096-%28-1">Exchange 2016 4096 (0x1000) is an invalid culture identifier</span>

#### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="http://www.kraeg.de/2017/10/30/exchange-2016-4096-0x1000-is-an-invalid-culture-identifier/" rel="nofollow">http://www.kraeg.de/2017/10/30/exchange-2016-4096-0x1000-is-an-invalid-culture-identifier/</a>
```

### <span class="mw-headline" id="bkmrk-exchange-2016-event--1">Exchange 2016 Event ID 1010 Fast Search not installed or Corrupted</span>

#### <span class="mw-headline" id="bkmrk-beschreibung%3A-1">Beschreibung:</span>

Benutzer bekommen keine Ergebnisse bei einer online Suche gegen den Microsoft Exchange Server  
Die Datenbanken auf dem die Mailbox ist haben beim **Index** den Status auf **Unknown**  
In der Ereignisanzeige am Server erscheint unter dem **Anwendungs** Event Log **Error ID 1010**

#### <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

1. Folgende Dienste beenden: 
    - Microsoft Exchange Search
    - Microsoft Exchange Search Host Controller
    
    <dl><dd>PowerShell administrativ öffnen 
    - `stop-service MSExchangeFastSearch`
    - `stop-service HostControllerService`
    
    </dd></dl>
2. Alle vier Ordner unter folgendem Pfad löschen: <dl><dt>`C:\Program Files\Microsoft\Exchange Server\V15\Bin\Search\Ceres\HostController\Data\Nodes\Fsis`</dt></dl>
3. In der Windows PowerShell zu folgendem Pfad wechseln: <dl><dt>`C:\Program Files\Microsoft\Exchange Server\V15\Bin\Search\Ceres\Installer`</dt></dl>
4. Folgenden Befehl ausführen: <dl><dt>`./installconfig.ps1 -action I -datafolder "c:\Program Files\Microsoft\Exchange Server\V15\Bin\Search\Ceres\HostController\Data"`</dt></dl>
5. Folgende Dienste wieder starten: 
    - Microsoft Exchange Search
    - Microsoft Exchange Search Host Controller
    
    <dl><dd>PowerShell administrativ öffnen 
    - `start-service MSExchangeFastSearch`
    - `start-service HostControllerService`
    
    </dd></dl>

#### <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="https://infosham.com/BLOG/exchange-server-2016-fast-search-errors" rel="nofollow">https://infosham.com/BLOG/exchange-server-2016-fast-search-errors</a>
```