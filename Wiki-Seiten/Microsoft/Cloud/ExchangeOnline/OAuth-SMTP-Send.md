---
title: OAuth SMTP Send
description: SMTP senden über eine Entra App die Exchange API Berechtigung hat
published: true
date: 2026-08-25T17:27:15.951Z
tags: exchange, office365, e-mail, oauth, passwordless
editor: markdown
dateCreated: 2026-08-25T17:20:08.148Z
---

# Zielbild
Folgende Herausforderung hatte ich in der Arbeit, es sollte möglich sein ohne Lizenzen zu verschwenden, E-Mails über SMTP
zu versenden und das ohne die Möglichkeit ein Connector zu verwenden oder direkt mit Graph zu arbeiten.

Bei uns war es nötig für ein SAP-Submodul, welches danach ungefähr so konfiguriert werden sollte:

SMTP Server: smtp.office365.com
Port: 587
Verschlüsselung: STARTTLS
Authentication: OAuth2ClientCredentials
Tenant ID: <Directory Tenant ID>
Client ID: <Application Client ID>
Client Secret oder Zertifikat: <Secret/Zertifikat>
Scope: https://outlook.office365.com/.default
Username / Sender / Shared-Mailbox: sap-smtp@deine-domain.de


Wichtig ist: Bei OAuth2ClientCredentials meldet sich keine Benutzerperson mit Passwort an. Die App holt ein Token über client_id und client_secret oder Zertifikat. Für SMTP muss das Token dann per SASL XOAUTH2 verwendet werden. Microsoft beschreibt für SMTP den Befehl AUTH XOAUTH2 mit einem Base64-kodierten String aus Benutzername und Bearer Token.

# Voraussetzungen

Du brauchst:

Global Admin, Privileged Role Admin oder passende Entra-Rechte für App Registration und Admin Consent, da keine Graph Rechte mit Admin Consent benötigt werden reicht die **Application Administrator** Role.

  Exchange Online Admin oder entsprechende Exchange-Rollen für New-ServicePrincipal, Add-MailboxPermission und Add-RecipientPermission.
  Eine Absender-Mailbox, zum Beispiel:
  sap-smtp@deine-domain.de

Exchange Online PowerShell Modul auf deinem Admin-PC.
SAP muss OAuth2ClientCredentials wirklich können, inklusive Token-Anforderung und XOAUTH2 SMTP-Authentifizierung. Microsoft verlangt für SMTP OAuth die Verwendung von XOAUTH2 am SMTP-Protokoll.
# Durchführung
## Teil 1: App Registration grafisch anlegen
1. Entra Admin Center öffnen
  Öffne:
    - [https://entra.microsoft.com](https://entra.microsoft.com){target=_blank}<br>
    Gehe zu: Identity > Applications > App registrations > New registration
    >  Microsoft verlangt für OAuth mit IMAP, POP oder SMTP eine registrierte Microsoft-Entra-Anwendung.
  {.is-info}
  
    ---

2. Neue App registrieren
  Empfohlene Werte:
    - Name: SAP SMTP OAuth Send
    - Supported account types: Accounts in this organizational directory only<br>
    Redirect URI: leer lassen
    >  Für Client Credentials brauchst du normalerweise keine Redirect URI, weil kein interaktiver Benutzer-Login erfolgt.
  {.is-info}
    
    Danach auf Register klicken.
  
3. IDs notieren
    Auf der Übersichtsseite der App notierst du:
    - Application (client) ID
    - Directory (tenant) ID
  
    Diese beiden Werte brauchst du später im SAP-Modul.
  
## Teil 2: Client Secret oder Zertifikat erstellen
**Empfehlung**
Wenn SAP Zertifikate unterstützt: Zertifikat bevorzugen.
Wenn SAP nur Secret unterstützt: Client Secret verwenden, aber Ablaufdatum dokumentieren und rechtzeitig erneuern.

- Variante A: Client Secret
  In der App Registration öffnen:
  Certificates & secrets > Client secrets > New client secret<br>
  Beschreibung setzen, zum Beispiel: SAP SMTP OAuth Secret <br>
  Ablaufzeit wählen.<br>
  Secret erstellen.<br>
  Value sofort kopieren, nicht nur die Secret ID.

> Wichtig: Der Secret Value wird später nicht erneut vollständig angezeigt.
{.is-warning}
  
### Notieren:
- Client Secret Value
- Nicht verwechseln mit:
- Secret ID
- Das SAP-Modul braucht in der Regel den Secret Value.

## Teil 3: API-Berechtigung SMTP.SendAsApp setzen
1. API Permissions öffnen
In der App Registration:
API permissions > Add a permission

2. Office 365 Exchange Online auswählen
Dann:
APIs my organization uses > Office 365 Exchange Online
> Microsoft nennt für SMTP im Client Credentials Flow die Application Permission SMTP.SendAsApp.

3. Application Permission auswählen
Auswählen:
Application permissions > SMTP.SendAsApp<br>
Danach:
Add permissions<br>
Nicht Delegated permissions verwenden. Für Option A brauchst du Application permissions, weil kein Benutzer interaktiv angemeldet wird. Microsoft unterscheidet hier ausdrücklich Application Permissions für Client Credentials.
<br>
4. Admin Consent erteilen
Klicke:
Grant admin consent for `<Tenantname>`<br>
Für eine Single-Tenant-App kannst du den Admin Consent direkt über die App-Konfigurationsseite im Entra Admin Center erteilen. Microsoft schreibt, dass bei Apps im eigenen Tenant dafür nicht der Admin-Consent-URL-Weg erforderlich ist.
Nachher sollte bei der Berechtigung stehen:
Granted for `<Tenantname>`

## Teil 4: Enterprise Application Object ID ermitteln

Das ist ein häufiger Fehlerpunkt.

Du brauchst später für New-ServicePrincipal:

- Application (client) ID
- Enterprise Application Object ID


Nicht verwenden:

- Object ID aus App registrations


Microsoft weist ausdrücklich darauf hin, dass die Object ID aus Enterprise applications verwendet werden muss, nicht die Object ID aus App registrations, weil sonst Authentifizierungsfehler entstehen können.

Grafisch ermitteln
Entra Admin Center öffnen.
Gehe zu:
Identity > Applications > Enterprise applications

Suche nach deiner App:
SAP SMTP OAuth Send

App öffnen.
Auf der Übersichtsseite notieren:
- Object ID
- Application ID


Die Application ID sollte identisch sein mit der Application client ID aus der App Registration.
Die Object ID ist aber eine andere und genau diese brauchst du für Exchange.

## Teil 5: Exchange Online PowerShell vorbereiten

Ab hier brauchst du PowerShell.

1. PowerShell als Admin oder normaler User starten
Am besten PowerShell 7 oder Windows PowerShell verwenden.

2. Modul installieren
    ```powershell
    Install-Module ExchangeOnlineManagement -Scope CurrentUser
    ```
    Falls Repository-Abfrage kommt, mit Y bestätigen.
    <br>
3. Modul laden
    ```powershell
    Import-Module ExchangeOnlineManagement
    ```

4. Mit Exchange Online verbinden
    ```powershell
    Connect-ExchangeOnline -Organization <tenant-id-oder-domain>
    ```

Beispiel:
```powershell
Connect-ExchangeOnline -Organization contoso.onmicrosoft.com
```

oder:
```powershell
Connect-ExchangeOnline -Organization 11111111-2222-3333-4444-555555555555
```

Microsoft zeigt für die Registrierung des Service Principals ebenfalls Install-Module ExchangeOnlineManagement, Import-Module ExchangeOnlineManagement und Connect-ExchangeOnline.

## Teil 6: Service Principal in Exchange Online registrieren
Jetzt registrierst du die Entra App zusätzlich in Exchange Online.
Werte vorbereiten

Du brauchst:
AppId:
Application (client) ID aus App Registration

ObjectId:
Object ID aus Enterprise Application

DisplayName:
frei wählbar, z.B. EXO-SP-SAP-SMTP-OAuth

Befehl
```powershell
New-ServicePrincipal `
  -AppId "<APPLICATION_CLIENT_ID>" `
  -ObjectId "<ENTERPRISE_APPLICATION_OBJECT_ID>" `
  -DisplayName "EXO-SP-SAP-SMTP-OAuth"
```

Beispiel:
```powershell
New-ServicePrincipal `
  -AppId "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" `
  -ObjectId "11111111-2222-3333-4444-555555555555" `
  -DisplayName "EXO-SP-SAP-SMTP-OAuth"
```

Microsoft beschreibt die Registrierung mit New-ServicePrincipal -AppId <application_id> -ObjectId <object_id> und erklärt, dass die Object ID aus der Enterprise Application verwendet werden muss.

Kontrolle
Get-ServicePrincipal -Identity "EXO-SP-SAP-SMTP-OAuth" | Format-List


Falls der Name nicht gefunden wird:
```powershell
Get-ServicePrincipal | Where-Object {$_.DisplayName -like "*SAP*"} | Format-List
```
## Teil 7: App auf die konkrete Absender-Mailbox berechtigen
Nehmen wir als Beispiel:

`sap-smtp@deine-domain.de`

Microsoft beschreibt, dass die App nach der Service-Principal-Registrierung auf konkrete Mailboxen berechtigt werden muss, zum Beispiel mit Add-MailboxPermission.

1. Exchange Service Principal Objekt holen
    ```powershell
    $ExoSp = Get-ServicePrincipal -Identity "EXO-SP-SAP-SMTP-OAuth"
    ```
    Kontrolle:
    ```powershell
    $ExoSp | Format-List
    ```
2. FullAccess auf die Absender-Mailbox setzen
    ```powershell
    Add-MailboxPermission `
      -Identity "sap-smtp@deine-domain.de" `
      -User $ExoSp.Identity `
      -AccessRights FullAccess
    ``` 
    Microsoft zeigt für die Freigabe einer Mailbox an eine App den Befehl `Add-MailboxPermission -Identity "<mailbox>" -User <service_principal_id> -AccessRights FullAccess`.

3. SendAs auf die Absender-Mailbox setzen
    Für SMTP-Senden als diese Mailbox empfehle ich zusätzlich explizit SendAs:
    ```powershell
    Add-RecipientPermission `
      -Identity "sap-smtp@deine-domain.de" `
      -Trustee $ExoSp.Identity `
      -AccessRights SendAs
     ```
    Microsoft weist ausdrücklich darauf hin, dass bei Client Credential Grant Flow mit SendAs die SendAs-Berechtigung für den Sender vergeben werden muss.

    Wenn eine Bestätigung kommt, mit Y bestätigen.

## Teil 8: SMTP AUTH für die Mailbox prüfen

Auch mit OAuth läuft der Versand über SMTP AUTH. Microsoft beschreibt SMTP AUTH als Protokoll für Client-SMTP-Übermittlung, typischerweise auf TCP-Port 587, und weist darauf hin, dass SMTP AUTH moderne Authentifizierung per OAuth unterstützt.

Tenantweite Einstellung prüfen
```powershell
Get-TransportConfig | Format-List SmtpClientAuthenticationDisabled
```

Interpretation:

False = SMTP AUTH tenantweit nicht blockiert
True  = SMTP AUTH tenantweit deaktiviert

Falls SMTP AUTH tenantweit deaktiviert ist und du es für diesen Weg brauchst:
```powershell
Set-TransportConfig -SmtpClientAuthenticationDisabled $false
```

Microsoft beschreibt, dass SMTP AUTH tenantweit über Set-TransportConfig -SmtpClientAuthenticationDisabled aktiviert oder deaktiviert werden kann.

Postfach-Einstellung prüfen
```powershell
Get-CASMailbox -Identity "sap-smtp@deine-domain.de" |
  Format-List SmtpClientAuthenticationDisabled
```

Interpretation:

False = SMTP AUTH für dieses Postfach erlaubt
True  = SMTP AUTH für dieses Postfach deaktiviert
Blank = keine explizite Postfachvorgabe, Tenant-Einstellung greift

Falls du es explizit für die Mailbox erlauben willst:
```powershell
Set-CASMailbox `
  -Identity "sap-smtp@deine-domain.de" `
  -SmtpClientAuthenticationDisabled $false
```

Microsoft beschreibt, dass es eine organisationsweite SMTP-AUTH-Einstellung und eine Postfach-Einstellung gibt, wobei die Postfach-Einstellung die mandantenweite Einstellung überschreiben kann.

## Teil 9: Werte für SAP eintragen
In SAP brauchst du je nach Maske ungefähr diese Werte:

Authentication:
OAuth2ClientCredentials

SMTP Server:
smtp.office365.com

SMTP Port:
587

TLS:
STARTTLS / TLS enabled

Tenant ID:
`<Directory tenant ID>`

Client ID:
`<Application client ID>`

Client Secret:
`<Client secret value>`

Token URL:
`https://login.microsoftonline.com/<TENANT_ID>/oauth2/v2.0/token`

Scope:
`https://outlook.office365.com/.default`

User / Mailbox / Sender:
`sap-smtp@deine-domain.de`


Für SMTP im Client Credentials Flow muss als Scope `https://outlook.office365.com/.default` verwendet werden. Microsoft nennt diesen Scope explizit für SMTP.

Falls SAP zusätzlich einen „Resource“-Wert statt Scope verlangt, ist das häufig:
`https://outlook.office365.com`

Das hängt aber von der SAP-Implementierung ab.

## Teil 10: Test und typische Fehler
Fehler: 535 5.7.3 Authentication unsuccessful

Mögliche Ursachen:
- Falsche Enterprise Application Object ID bei New-ServicePrincipal.
- Admin Consent fehlt.
- Falscher Scope.
- Secret falsch, abgelaufen oder Secret ID statt Secret Value eingetragen.
- SAP sendet kein echtes XOAUTH2.
- SMTP AUTH auf Tenant oder Mailbox blockiert.

Microsoft zeigt bei SMTP OAuth als Fehlerbeispiel ebenfalls `535 5.7.3 Authentication unsuccessful`.

Fehler: „not permitted to send as this user“

Dann fehlt meist SendAs:
```powershell
Add-RecipientPermission `
  -Identity "sap-smtp@deine-domain.de" `
  -Trustee $ExoSp.Identity `
  -AccessRights SendAs
```

Microsoft weist für Client Credentials mit SendAs explizit auf Add-RecipientPermission hin.

Fehler trotz korrekter Berechtigungen direkt nach Einrichtung

Wartezeit einplanen. In Exchange Online können Berechtigungen und Service-Principal-Änderungen etwas brauchen, bis alle Backends die Änderung kennen. Ich würde nach der Konfiguration mindestens 15 bis 60 Minuten einplanen, bei hartnäckigen Fällen auch länger.

Komplette PowerShell-Blöcke zum Kopieren
```powershell
Variante mit Platzhaltern
# Verbindung herstellen
Import-Module ExchangeOnlineManagement
Connect-ExchangeOnline -Organization "<TENANT_ID_OR_DOMAIN>"

# Service Principal in Exchange registrieren
New-ServicePrincipal `
  -AppId "<APPLICATION_CLIENT_ID>" `
  -ObjectId "<ENTERPRISE_APPLICATION_OBJECT_ID>" `
  -DisplayName "EXO-SP-SAP-SMTP-OAuth"

# Service Principal holen
$ExoSp = Get-ServicePrincipal -Identity "EXO-SP-SAP-SMTP-OAuth"

# Mailbox berechtigen
Add-MailboxPermission `
  -Identity "sap-smtp@deine-domain.de" `
  -User $ExoSp.Identity `
  -AccessRights FullAccess

# SendAs berechtigen
Add-RecipientPermission `
  -Identity "sap-smtp@deine-domain.de" `
  -Trustee $ExoSp.Identity `
  -AccessRights SendAs

# SMTP AUTH prüfen
Get-TransportConfig | Format-List SmtpClientAuthenticationDisabled

Get-CASMailbox -Identity "sap-smtp@deine-domain.de" |
  Format-List SmtpClientAuthenticationDisabled
```
  
Falls SMTP AUTH für die Mailbox explizit aktiviert werden muss
```powershell
Set-CASMailbox `
  -Identity "sap-smtp@deine-domain.de" `
  -SmtpClientAuthenticationDisabled $false
```
Falls SMTP AUTH tenantweit blockiert ist und du es bewusst erlauben willst
```powershell
Set-TransportConfig -SmtpClientAuthenticationDisabled $false
```
**Wichtige Sicherheitsentscheidung**

Ich würde die App nur auf genau eine dedizierte Mailbox berechtigen, zum Beispiel:

`sap-smtp@deine-domain.de`


Nicht auf Benutzerpostfächer von Personen. Dann kann man Versand, Audit, Berechtigungen und spätere Rotation sauber steuern.

Außerdem empfehle ich:
- Client Secret mit Ablaufdatum dokumentieren.
- Wenn möglich Zertifikat statt Secret verwenden.
- App nur mit **SMTP.SendAsApp**, nicht zusätzlich mit IMAP oder POP berechtigen.
- Berechtigungen regelmäßig prüfen.
- Absender-Mailbox bewusst benennen, zum Beispiel `sap-noreply@domain.de` oder `sap-system@domain.de`.
