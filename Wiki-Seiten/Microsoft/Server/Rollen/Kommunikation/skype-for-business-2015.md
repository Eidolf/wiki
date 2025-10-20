---
title: skype-for-business-2015
description: 
published: true
date: 2023-12-27T12:21:59.895Z
tags: 
editor: markdown
dateCreated: 2023-12-10T10:19:00.479Z
---

# Skype for Business 2015

## <span class="mw-headline" id="bkmrk-installation-1">Installation</span>

### <span class="mw-headline" id="bkmrk-in-place-upgrade-1">In Place Upgrade</span>

Es gibt schon wirklich gut Artikel, daher nur die entsprechenden Links dazu. Hinweis! Alle Voraussetzungen für die Installation von Skype for Business sind wirklich genau zu befolgen, die Installation startet sonst nicht, obwohl man bei 32GB freien speicher und DotNet Framework 3.5 schon mal überlegt ob man das bereitstellt. 1. In diesem Link ist die SQL Voraussetzung gut beschrieben und ein Script zur Versionsabfrage

```
https://markvale.wordpress.com/2015/05/02/skype-for-business-server-in-place-upgrade-step-by-step/
```

2\. Hier das FrontEnd Hauptupgrade

```
http://windowspbx.blogspot.de/2015/04/step-by-step-skype-for-business-server.html
```

3\. Zum Schluss noch das Edge Server Upgrade

```
http://www.haveyourebooted.com/upgrade-lync-2013-edge-server-to-skype-for-business/
```

### <span class="mw-headline" id="bkmrk-exchange-2013-integr-1">Exchange 2013 Integration</span>

1. Cd $exscripts <dl><dt>`.\Configure-EnterprisePartnerApplication.ps1 –AuthMetadataURL <a class="external free" href="https://skypefe.fqdn/metadata/jason/1" rel="nofollow">https://SkypeFE.fqdn/metadata/jason/1</a> -ApplicationType “Lync”`</dt></dl>
    - Erstellt einen Account im AD "LyncEnterprise-ApplicationAccount"
    - Erstellt RBAC Berechtigungen.
    
    <dl><dt>Prüfbar über</dt><dd>`Get-ManagementRoleAssignment -Roleassignee LyncEnterprise-ApplicationAccount`</dd></dl>
    - Erstellt Partner Anwendungen.
    
    <dl><dt>Prüfbar über</dt><dd>`Get-PartnerApplication Lync* | fl`</dd></dl>
2. Autodiscover URL abrufen `Get-ClientAccessServer | Select Name,*uri*`
3. S4B Powershell `New-CSPartnerApplication –Identity Exchange –ApplicationTrustLevel Full –MetadataURL <a class="external free" href="https://webmail.fqdn/autodiscover/metadata/json/1" rel="nofollow">https://webmail.fqdn/autodiscover/metadata/json/1</a>`
4. Erster Funktionstest `Test-CSEXStorageConnectivity`
5. Exchange Zertifikat dem S4B Server als vertrauenswürdig darstellen 
    1. zu Pfad wechseln **C:\\Program Files\\Microsoft\\Exchange Server\\V15\\ClientAccess\\Owa**
    2. web.config öffnen
    3. unter *&lt;appSettings&gt;* folgende Einträge erstellen 
        - &lt;add key=”IMCertificateThumbprint” value=”Exchange ThumbPrint” /&gt;
        - &lt;add key=”IMServerName” value=”S4B-FQDN” /&gt;
6. OWA Integration 
    1. Herausfinden ob Unified Communications Managed API 4.0 installiert ist 
        - `Get-Item “hklm:\system\CurrentControlSet\Services\MSExchange OWA\InstantMessaging”`
    2. Setzen der Einstellungen <dl><dt>`Get-OWAVirtualDirectory | Set-OWAVirtualDirectory –InstantMessagingType OCS`</dt><dt>`Get-OWAVirtualDirectory | Ft Name,InstantMessagingEnabled,InstantMessagingType –AutoSize`</dt><dt>`Get-OWAMailboxPolicy | ft Name,InstantMessagingEnabled,InstantMessagingType -AutoSize`</dt><dt>`Get-OWAMailboxPolicy | Set-OWAMailboxPolicy -InstantMessagingType OCS`</dt></dl>
    3. CMD öffnen <dl><dt>`Cd \Windows\system32\inetsrv`</dt><dt>`Appcmd.exe recycle apppool /apppool.name:”MSExchangeOWAAppPool”`</dt><dt>Eventuell noch einen iisreset /noforce</dt></dl>
7. S4B Trusted Application erstellen 
    1. Site herausfinden und für nächsten Befehl merken `Get-CSSite`
    2. Pool erstellen <dl><dt>`New-CSTrustedApplicationPool –Identity <Exchange-FQDN> -Registrar <Skye-for-Business-FQDN> -Site <Site-Name> -RequiresReplication $False`</dt></dl>
    3. Applikation erstellen <dl><dt>`New-CSTrustedApplication –ApplicationID EXOWA –TrustedApplicationPoolFQDN <Exchange-FQDN> -Port <Random-High-Port>`</dt></dl>
    4. Topologie veröffentlichen `Enable-CSTopology`

#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.msexchange.org/articles-tutorials/exchange-server-2013/migration-deployment/integrating-exchange-server-2013-and-skype-business-server-2015-part1.html" rel="nofollow">http://www.msexchange.org/articles-tutorials/exchange-server-2013/migration-deployment/integrating-exchange-server-2013-and-skype-business-server-2015-part1.html</a>
```

### <span class="mw-headline" id="bkmrk-exchange-2016-integr-1">Exchange 2016 Integration</span>

In der Quelle ist eine gute Beschreibung zu finden für die komplette Neuintegration, mein folgender Hinweis bezieht sich nur auf ein Update der bestehenden Integration (explizit OWA Status) nach einem Zertifikatstausch.

1. Auf den Exchange Server verbinden
2. Administrative Exchange PowerShell öffnen
3. Den aktuellen Thumbprint des Exchange Server Authentication Zertifikats (z.B. bei UM Rolle genutzt) kopieren `Get-ExchangeCertificate`
4. Name des vorhandenen Konfiguration herausfinden `Get-SettingOverride`
5. Die neuen Einstellungen setzen `Set-SettingOverride -Identity "VorhandenerNameDerConfig" -Parameters @(“IMServerName=FQDN S4B Frontend”,”IMCertificateThumbprint=vorher kopierter Thumbprint”)`
6. Konfiguration mit folgendem Befehl aktualisieren `Get-ExchangeDiagnosticInfo -Server $ENV:COMPUTERNAME -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh`
7. WebAppPool neu starten `Restart-WebAppPool MSExchangeOWAApppool`

#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://lyncdude.com/2015/10/06/the-complete-skype-for-business-exchange-2016-integration-guide-part-i/index.html" rel="nofollow">http://lyncdude.com/2015/10/06/the-complete-skype-for-business-exchange-2016-integration-guide-part-i/index.html</a>
```

#### <span class="mw-headline" id="bkmrk-fehler-500-bei-neuei-1">Fehler 500 bei Neueinrichtung</span>

Falls folgender Fehler erscheint nachdem man versucht den Exchange als Partnerapplikation hinzuzufügen  
![NewCSPartnerApp-Error500.png](/media/NewCSPartnerApp-Error500.png)
Kann man über einen modernen Browser testen ob das Signatur Zertifikat ungültig geworden ist, beim IE wird einfach nur ein Fehler 500 angezeigt.  
![SignaturZertUngueltig.png](/media/SignaturZertUngueltig.png)  
Der angezeigte Fingerabdruck ist von einem Zertifikat das auf dem Exchange Server nicht mehr vorhanden ist.  
Mit `Get-AuthConfig` kann man diesen hinterlegten (falschen) Wert über die Exchange Shell nochmal gegenprüfen.  
Um das Zertifikat zu aktualisieren muss man folgende Punkte durchgehen.

1. `Get-ExchangeCertificate`, hier den Fingerabdruck des "Microsoft Exchange Server Auth" Zertifikats finden und kopieren
2. `$a = Get-Date`
3. `Set-AuthConfig -NewCertificateThumbprint KopierterFingerabdruck -NewCertificateEffectiveDate $a`
4. `Set-AuthConfig -PublishCertificate`
5. `Set-AuthConfig -ClearPreviousCertificate`
6. `iisreset`

##### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/de-de/archive/blogs/jenstr/getting-internal-server-error-500-when-creating-new-cspartnerapplication-for-exchange-2013" rel="nofollow">https://docs.microsoft.com/de-de/archive/blogs/jenstr/getting-internal-server-error-500-when-creating-new-cspartnerapplication-for-exchange-2013</a>
```

## <span class="mw-headline" id="bkmrk-einstellungen-1">Einstellungen</span>

### <span class="mw-headline" id="bkmrk-mediation-server-1">Mediation Server</span>

[S4B Mediation Server Einrichten](https://wiki.eidolf.de/index.php/S4B_Mediation_Server_Einrichten "S4B Mediation Server Einrichten")

### <span class="mw-headline" id="bkmrk-offline-nachrichten-1">Offline Nachrichten</span>

#### <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="https://support.office.com/en-us/article/Turn-on-or-off-Offline-Messages-for-admins-8967a77f-caa2-4680-aa22-8faa32c716e4?ui=en-US&rs=en-US&ad=US" rel="nofollow">https://support.office.com/en-us/article/Turn-on-or-off-Offline-Messages-for-admins-8967a77f-caa2-4680-aa22-8faa32c716e4?ui=en-US&rs=en-US&ad=US</a>
```