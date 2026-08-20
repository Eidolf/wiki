---
title: Exchange Online Allgemein
description: Informationen zu Exchange Online
published: true
date: 2026-08-20T16:43:40.456Z
tags: exchange, office365, e-mail
editor: markdown
dateCreated: 2024-05-28T15:58:01.363Z
---

# Microsoft forciert onPrem Exchange Version
Zur Absicherung forciert Exchange Online gewisse Mindestversionen von onPrem Exchange Servern in Hybrid Umgebungen.
Zum Stand 28.05.2024 sind es alle Versionen >= 15.01.2507.031 (Exchange Server 2016 CU23 Aug23SU).
Folgende Meldung erscheint im Exchange Admin Center unter Reporting
![exchange-block-old-onprem.png](/media/exchange-block-old-onprem.png)

Falls es keine Möglichkeit gibt den Exchange auf den aktuellen Stand zu bekommen kann man im Exchange Admin Center unter Reporting eine Pausierung dieser Funktion aktivieren.
Microsoft lässt pro Jahr nur gesamt 90 Tage an Verzögerung zu.

Einstellungen zur Pausierung ist hier zu finden:
EAC > Reports > Mail flow > Out-of-date connecting on-premises Exchange servers > Enforcement Pause
![exchange-out-of-date-connection_001.png](/media/exchange-out-of-date-connection_001.png)

![exchange-out-of-date-connection_002.png](/media/exchange-out-of-date-connection_002.png)

![exchange-out-of-date-connection_003.png](/media/exchange-out-of-date-connection_003.png)

### Quelle:
https://techcommunity.microsoft.com/t5/exchange-team-blog/how-to-pause-throttling-and-blocking-of-out-of-date-on-premises/ba-p/4007169
https://practical365.com/microsoft-block-old-exchange-servers/

# SMTP Send über Office 365
Microsoft versucht mit allen mitteln unsichere Verbindunge zu unterbinden, was eindeutig die richtige Richtung ist.
Aber es gibt leider einzelne Abläufe wo man dies übergehen möchte.
> Folgende Einstellungen würde ich nicht in einem Firmenumfeld einrichten
{.is-danger}

## Ausgangslage
Wie in der Warnung zu entnehmen ist, habe ich diese Anleitung nur für eine private Problematik geschrieben.
Ich wollte von der Fritzbox, Nachrichten versenden da über mein Domainanbieter DKIM erforderlich geworden wäre und dieser keine Möglichkeit hatte dies einzurichten.
Bei Office 365 SMTP bin ich leider erstmal auch auf die Sicherheitsrichtlinien gestoßen und deshalb folgende Einrichtung.

## Einrichtung
1. Ein Postfach mit Exchange Lizenz sollte vorhanden sein bei der man sich mit Benutzername und Passwort authentifizieren kann.
2. Im Microsoft Admin Portal unter den [Benutzern](https://admin.cloud.microsoft/#/users){target=_blank} sucht man sich das Postfach zum senden heraus
3. Im Fly Out Menü klickt man auf den **"E-Mail"** Reiter
4. Im **"E-Mail"** Reiter klickt man auf **"E-Mail-Apps verwalten"** und hier markiert man **"Authentifiziertes SMTP"**
![exchange-authentifiziertes-smtp-001.png](/media/exchange-authentifiziertes-smtp-001.png)
5. Ab jetzt muss man in das  [EntraID Portal > Overview](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/TenantOverview.ReactView/initialValue//tabId//recommendationResourceId//fromNav/Identity){target=_blank} wechseln
6. Dort auf den **"Einstellungen"** Reiter und prüfen ob Security defaults aktiviert sind, falls nicht zu Punkt 7 (wie bei mir) 
![exchange-authentifiziertes-smtp-002.png](/media/exchange-authentifiziertes-smtp-002.png)
7. Conditional Access Policies öffnen oder wie im voherigen Screenshot sichtbar auf [Manage Conditional Access](https://entra.microsoft.com/#view/Microsoft_AAD_ConditionalAccess/PoliciesList.ReactView){target=_blank} klicken.
8. Bei mir gibt es eine extra Policy die legacy sign-ins blockt und somit die einzige Stelle für eine Ausnahme ist.
9. In der Conditional Access Poliy unter **"Benutzer oder Agenten"** auf **"Ausnahmen"** und dort den Benutzer bei "Benutzer und Gruppen" hinzufügen. 
![exchange-authentifiziertes-smtp-003.png](/media/exchange-authentifiziertes-smtp-003.png)
10. Ab dem Zeitpunkt konnte ich von meiner Fritzbox E-Mails mit der angegebenen Mailbox versenden.
## Quelle:
https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/authenticated-client-smtp-submission

# Hybrid Migration Endpoint
## Kann nicht neu eingerichtet werden bzw. der vorhandene läuft auf einen Fehler

### Fehlerbild

Beim Test eines Exchange-Hybrid-Migration-Endpoints treten nacheinander folgende Fehler auf:

```
The connection to the server could not be completed.
```

Beim direkten Aufruf des MRS-Proxy-Endpunkts:
```
HTTP 500 Internal Server Error
Missing signing certificate.
```

Nach Behebung des Zertifikatsfehlers:
```
The HTTP request is unauthorized with client authentication scheme 'Negotiate'.
The authentication header received from the server was 'NTLM, Negotiate'.
```

Betroffener Endpunkt:
```
https://<Exchange-FQDN>/EWS/mrsproxy.svc
```

### Ursache

Es lagen zwei unabhängige Zertifikatsprobleme vor:

1. Das in Get-AuthConfig eingetragene aktuelle Exchange Auth Certificate war auf dem Exchange Server nicht mehr vorhanden. Ein gültiges Zertifikat war bereits als NextCertificateThumbprint hinterlegt, jedoch noch nicht veröffentlicht.

2. Der vorgeschaltete Reverse Proxy und der Exchange-IIS verwendeten unterschiedliche TLS-Zertifikate. Bei aktivierter Extended Protection führte das SSL Bridging dadurch zu einer fehlerhaften Channel-Binding-Prüfung und zur Ablehnung der NTLM-/Negotiate-Authentifizierung.

### Lösung
1. **Exchange Auth Certificate veröffentlichen**
    Konfiguration prüfen:
    
    ```powershell
    Get-AuthConfig |
        Format-List CurrentCertificateThumbprint,
                    PreviousCertificateThumbprint,
                    NextCertificateThumbprint
    ```
    
    Das unter NextCertificateThumbprint hinterlegte Zertifikat prüfen:

    ```powershell
    \$Thumbprint = (Get-AuthConfig).NextCertificateThumbprint

    Get-ExchangeCertificate -Thumbprint \$Thumbprint |
        Format-List Thumbprint,NotAfter,Status,HasPrivateKey
    ```
    
    Wenn das Zertifikat gültig ist und einen privaten Schlüssel besitzt:
    
    ```powershell
    Set-AuthConfig -PublishCertificate
    Restart-Service MSExchangeServiceHost
    Restart-WebAppPool MSExchangeServicesAppPool
    ```

2. **MRS Proxy aktivieren**
    ```powershell
    Get-WebServicesVirtualDirectory |
        Set-WebServicesVirtualDirectory -MRSProxyEnabled $true

    Restart-WebAppPool MSExchangeServicesAppPool
    ```

3. **Identisches TLS-Zertifikat verwenden**
    Bei HTTPS SSL Bridging muss der Reverse Proxy dasselbe Zertifikat präsentieren, das auch am Exchange-IIS gebunden ist.
    
    Das extern verwendete Zertifikat auf dem Exchange Server für IIS aktivieren:
    ```powershell
    Enable-ExchangeCertificate `
        -Thumbprint "<Zertifikatthumbprint>" `
        -Services IIS
    ```
    Extended Protection anschließend auf dem EWS Virtual Directory aktiv lassen:
    ```powershell
    Set-WebServicesVirtualDirectory `
        -Identity "<Server>\EWS (Default Web Site)" `
        -ExtendedProtectionTokenChecking Allow

    Restart-WebAppPool MSExchangeServicesAppPool
    ```

4. **Verbindung aus Exchange Online testen**
    ```powershell
    $Credential = Get-Credential

    Test-MigrationServerAvailability `
        -ExchangeRemoteMove `
        -RemoteServer "<Exchange-FQDN>" `
        -Credentials $Credential
    ```

Erwartetes Ergebnis:
```
Result : Success
```

### Ergebnis

Der vorhandene Migration Endpoint kann weiterverwendet werden. Ein erneuter Lauf des Hybrid Configuration Wizard oder die dauerhafte Deaktivierung von Extended Protection ist nicht erforderlich.

#### Technische Kernaussage

Bei einem Reverse Proxy mit HTTPS SSL Bridging müssen Proxy und Exchange-IIS dasselbe TLS-Zertifikat verwenden, damit die Channel-Binding-Prüfung von Extended Protection bei NTLM-/Negotiate-Authentifizierung erfolgreich ist.
