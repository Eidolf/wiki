---
title: Exchange 2016
description: 
published: true
date: 2025-10-24T15:40:07.658Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:32:54.175Z
---

# Installation

## Sprachpakete

Erste ein kurzer Hinweis wie man deinstalliert, wird bei Upgrades auf ein neues CU benötigt.

1. Exchange Installationsmedium einbinden
2. Zu eingebunden Medium in der Exchange Powershell wechseln
3. Deinstallation starten mit folgendem Befehl `.\setup.exe /removeumlanguagepack de-DE`

**Installation:**

1. Mit der Exchange Power Shell in den Pfad des Exchange Installationsmediums wechseln
2. Befehl ausführen
`Setup.com /AddUmLanguagePack:de-DE /s: H:\PfadzumUMPaket /IAcceptExchangeServerLicenseTerms`

## Server

1. DotNet 4.8 installieren
2. UCMA Runtime installieren
3. vcredist\_x64 von 2012 und 2013 installieren
4. Rollen mit Administrativer PowerShell installieren  
    `Install-WindowsFeature NET-WCF-HTTP-Activation45, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS`
5. Antivierenscanner für den Installationsvorgang kurzzeitig beenden.
6. Exchange Setup starten.

# Installation Hinweise

## Exchange 2016 in 2016 AD

Falls ein Exchange 2016 in einem 2016 AD installiert werden soll wird mindestens ein Installationsmedium **EX 2016 CU3** benötigt.

### Quelle:

https://social.technet.microsoft.com/Forums/azure/de-DE/abeb1b39-6d2d-4faf-9c24-36d3d45f9dc2/installing-exchange-2016-on-server-2016-error-with-gui?forum=Exch2016SD

# Sicherheit erhöhen

Persönliche Meinung: Ist bisher mit Vorsicht zu genießen, da Funktionen wie z.B. bei der S4B Integration nicht mehr funktionieren, wie z.B. Sprachverlauf am Smartphone.

## Quelle:

https://www.frankysweb.de/exchange-2016-installation-absichern-hardening/

# Update

Exchange CU Updates sind eigentlich ein komplettes neu installieren des Exchange Server's und in meiner Umgebung mit folgenden Punkten am Besten durchzuführen.

1. Deinstallieren des UM Sprachpakets siehe > [Exchange-2016 Sprachpakete](#sprachpakete)
2. Exchange Installationsmedium an einem DC einbinden. 
    1. PowerShell öffnen und folgende Befehle als Domain Admin / Organisations Admin durchführen 
        1. `E:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareSchema`
        2. `E:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareAD /OrganizationName:"<Organization name>"`
        OrganizationName ist zu finden über PowerShell am Exchange mit folgendem Befehl `Get-OrganizationConfig | select Name`
        3. `E:\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareAllDomains`
3. Installation des CU am Exchange Server
4. Installation Sprachpaket 
    1. Exchange Management Shell öffnen (Neuer Befehl ab CU21)
    2. `Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /AddUmLanguagePack:de-DE /SourceDir:"c:\install"`

# Hybrid
[Exchange-2016-Hybrid-Konfiguration](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-2016-hybrid-konfiguration)
Informationen zu Exchange Hybridszenarien in Verbindung mit O365

# Fehler:

## Exchange 2016 4096 (0x1000) is an invalid culture identifier

### Quelle:

http://www.kraeg.de/2017/10/30/exchange-2016-4096-0x1000-is-an-invalid-culture-identifier/

## Exchange 2016 Event ID 1010 Fast Search not installed or Corrupted

### >Beschreibung:

Benutzer bekommen keine Ergebnisse bei einer online Suche gegen den Microsoft Exchange Server  
Die Datenbanken auf dem die Mailbox ist haben beim **Index** den Status auf **Unknown**  
In der Ereignisanzeige am Server erscheint unter dem **Anwendungs** Event Log **Error ID 1010**

### Lösung:

1. Folgende Dienste beenden: 
    - Microsoft Exchange Search
    - Microsoft Exchange Search Host Controller
    
> PowerShell administrativ öffnen   
>	`stop-service MSExchangeFastSearch`
> `stop-service HostControllerService`
    
2. Alle vier Ordner unter folgendem Pfad löschen:
`C:\Program Files\Microsoft\Exchange Server\V15\Bin\Search\Ceres\HostController\Data\Nodes\Fsis`
3. In der Windows PowerShell zu folgendem Pfad wechseln:
`C:\Program Files\Microsoft\Exchange Server\V15\Bin\Search\Ceres\Installer`
4. Folgenden Befehl ausführen: 
`./installconfig.ps1 -action I -datafolder "c:\Program Files\Microsoft\Exchange Server\V15\Bin\Search\Ceres\HostController\Data"`
5. Folgende Dienste wieder starten: 
    - Microsoft Exchange Search
    - Microsoft Exchange Search Host Controller
    
> PowerShell administrativ öffnen 
> `start-service MSExchangeFastSearch`
> `start-service HostControllerService`

### Quelle:
https://infosham.com/BLOG/exchange-server-2016-fast-search-errors