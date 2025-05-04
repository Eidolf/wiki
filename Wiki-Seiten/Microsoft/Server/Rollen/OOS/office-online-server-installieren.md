---
title: office-online-server-installieren
description: 
published: true
date: 2025-05-04T11:43:00.026Z
tags: 
editor: markdown
dateCreated: 2023-12-31T11:55:44.885Z
---

# Office Online Server installieren

# Installation

1. Server 2016 Rollen installieren
	`Add-WindowsFeature Web-Server,Web-Mgmt-Tools,Web-Mgmt-Console,Web-WebServer,Web-Common-Http,Web-Default-Doc,Web-Static-Content,Web-Performance,Web-Stat-Compression,Web-Dyn-Compression,Web-Security,Web-Filtering,Web-Windows-Auth,Web-App-Dev,Web-Net-Ext45,Web-Asp-Net45,Web-ISAPI-Ext,Web-ISAPI-Filter,Web-Includes,NET-Framework-Features,NET-Framework-45-Features,NET-Framework-Core,NET-Framework-45-Core,NET-HTTP-Activation,NET-Non-HTTP-Activ,NET-WCF-HTTP-Activation45,Windows-Identity-Foundation,Server-Media-Foundation`
2. Installation folgender weiteren Voraussetzungen 
    1. [MicrosoftIdentityExtensions-64](https://go.microsoft.com/fwlink/p/?LinkId=620072)
    2. [vc\_redist.x64-2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)
    3. [vcredist\_x64-2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784)
3. Office Online Server installer ausführen (Am Besten die englische Version, da in der deutschen {Stand 09/2017} gefühlt ein Bug implementiert ist)
4. Am IIS ein Zertifikat anfordern und bei der Zertifizierungsstelle einreichen
5. Am IIS die Zertifikatsanforderung abschließen und unbedingt eine Namen angeben, siehe bei nachfolgendem Power Shell Befehl den "CertificateName"
6. Power Shell öffnen und nachfolgenden Befehl eingeben um eine Farm zu erstellen 
	`New-OfficeWebAppsFarm -InternalURL "https://oos.domäne.de" -ExternalURL "https://oos.domäne.de" -CertificateName "oos.domäne.de"`
  6.1. Mit folgendem Befehl kann online Editiert werden (Lizenz vorausgesetzt)
  `New-OfficeWebAppsFarm -InternalURL "https://oos.domäne.de" -ExternalURL "https://oos.domäne.de" -CertificateName "oos.domäne.de" -EditingEnabled`
7. Grober Test: Seite **https://oos.domäne.de/hosting/discovery** aufrufen, es sollte eine xml Datei angezeigt werden.
8. Ab jetzt müssen Exchange Einstellungen gesetzt werden

## Quelle:
https://docs.microsoft.com/de-de/officeonlineserver/deploy-office-online-server
https://jaapwesselius.com/2016/12/08/install-office-online-server-2016/
https://cloudmoran.wordpress.com/2016/08/22/deploy-office-online-server-owa-aka-wac-with-skype-for-business-server-2015/