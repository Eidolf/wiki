---
title: Exchange Allgemein
description: 
published: true
date: 2025-10-24T15:45:58.868Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:33:03.680Z
---

# Anonymous Relay

## Aktivieren

```
New-ReceiveConnector -Name "Anonymous Relay" -Usage Custom -PermissionGroups AnonymousUsers -Bindings ServerIP:25 -RemoteIPRanges AnonymerClient
```

```
Get-ReceiveConnector "Anonymous Relay" | Add-ADPermission -User "NT AUTHORITY\ANONYMOUS LOGON" -ExtendedRights "Ms-Exch-SMTP-Accept-Any-Recipient"
```

Bei einem deutschen AD muss der Benutzer gegen **NT-AUTORITÄT\\ANONYMOUS-ANMELDUNG** ersetzt werden.

## Abfragen

Allgemein Anonym:

```
Get-ReceiveConnector | Get-ADPermission | where {$_.User -like '*anonymous*'} | ft identity,user,extendedrights,accessrights
```

Eingrenzen auf Any Recipient:

```
Get-ReceiveConnector | Get-ADPermission | where {$_.User -like '*anonymous*' -and $_.ExtendedRights -like 'Ms-Exch-SMTP-Accept-Any-Recipient'} | ft identity,user,extendedrights,accessrights
```

## Exchange 2010 (älterer Wiki Eintrag)

[exchange-2010-zulassen-von-anonymem-relay-fur-einen-empfangsconnector](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-2010-zulassen-von-anonymem-relay-fur-einen-empfangsconnector)

# Datenbanken löschen

Insbesondere geht es in dieser Lösung um die erste Datenbank an einem Exchange Server in der System Mailboxen liegen.  
Falls die Exchange Server in verschiedenen Domänen sind kann es sein das nur mit folgendem Befehl alle System Mailboxen angezeigt werden.  
`set-adserversettings -viewentireforest $true`
Als nächstes kann man alle noch vorhandenen Mailboxen mit folgenden Befehlen überprüfen.

```
get-mailbox -Database "Mailbox Database"
get-mailbox -Database "Mailbox Database" -AuditLog
get-mailbox -Database "Mailbox Database" -Archive
get-mailbox -Database "Mailbox Database" -Arbitration
get-mailbox -Database "Mailbox Database" -PublicFolder
get-mailbox -Database "Mailbox Database" -RemoteArchive
get-mailbox -Database "Mailbox Database" -Monitoring
get-mailbox -Database "Mailbox Database" -AuxAuditLog
get-mailbox -Database "Mailbox Database" -Migration
get-mailbox -Database "Mailbox Database" -GroupMailbox
```

Falls irgendwo noch eine Mailbox zu finden ist kann man sie wie folgt verschieben.(Flag jeweils abändern, dies ist ein schematisches Beispiel)  
`get-mailbox -Database "Mailbox Database" -Arbitration | New-MoveRequest -TargetDatabase "DBSYS_Database_New"`  
Danach kann man die Datenbank mit folgendem Befehl löschen.  
`remove-mailboxdatabase "Mailbox Database"`

# Impersonation Rechte

Erweiterte AD Berechtigung um einem Dienstbenutzer Berechtigung auf Postfächer in deren Namen zu erteilen.

## Anwendungsbeispiele

### Berechtigung eines Service Accounts auf Shared Mailboxen in AD Gruppe

1. Management Scope erstellen (AD Gruppe auswählen) 
    1. Security Group im AD erstellen und als Mitglieder die Ziel Shared Mailboxen hinzufügen.
    2. **distinguishedName** aus dem ADUC Attribut Editor kopieren.
    3. Mit der Exchange Management Shell den Scope erstellen. <dl><dd>`New-ManagementScope -RecipientRestrictionFilter {((MemberOfGroup -eq "distinguishedName") -and (RecipientType -eq "UserMailbox"))}`</dd></dl>
2. Berechtigung zuweisen 
    1. Zur Exchange Management Konsole verbinden
    2. Unter Berechtigung &gt; Administrator Rollen auf das Plus Zeichen klicken <dl><dd>[![Exchange-Impersonation-001.png](https://wiki.eidolf.de/images/c/c9/Exchange-Impersonation-001.png)](https://wiki.eidolf.de/index.php/Datei:Exchange-Impersonation-001.png)</dd></dl>
    3. Hier wird die neue Rollengruppe mit den Impersonationrechten angelegt 
        1. Name der Rollengruppe ausfüllen
        2. Den zuvor angelegten Schreibbereich auswählen
        3. Die Impersonation Rechte auswählen
        4. Welcher Account die Berechtigung bekommt auswählen
        
        <dl><dd>[![Exchange-Impersonation-002.png](https://wiki.eidolf.de/images/c/cf/Exchange-Impersonation-002.png)](https://wiki.eidolf.de/index.php/Datei:Exchange-Impersonation-002.png)</dd></dl>

# Exchange ein Neustart ist ausstehend

Folgenden Regeintrag löschen:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\PendingFileRenameOperations
```

## Quelle:

```
<a class="external free" href="https://technet.microsoft.com/de-de/library/cc164360%28v=exchg.80%29.aspx" rel="nofollow">https://technet.microsoft.com/de-de/library/cc164360%28v=exchg.80%29.aspx</a>
```

# Exchange Empfangs Connector EHLO / HELO Server ändern

[exchange-helo-begrussung-andern](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-helo-begrussung-andern)

# Exchange Queue Pfad ändern

## Bestehende Queue verschieben

Auch für eine bestehende Queue kann man die unter **Neue Queue erstellen** beschriebene Version verwenden, doch gibt es ein Script das alle Aufgaben automatisch erledigt.


1. Eine Administrative Exchange PowerShell öffnen.
2. `cd $ExScripts`
3. Im Falle man möchte die gesamte Queue auf das **Laufwerk H:** verschieben verwendet man folgenden Code
		Auf dem Laufwerk H: müssen **keine Ordner** angelegt sein, diese werden durch das Script erstellt und berechtigt.
    `.\Move-TransportDatabase.ps1 -queueDatabasePath 'H:\TransportRoles\data\Queue' -queueDatabaseLoggingPath
    'H:\TransportRoles\data\Queue\Logs' -iPFilterDatabasePath 'H:\TransportRoles\data\IpFilter' -iPFilterDatabaseLoggingPath
    'H:\TransportRoles\data\IpFilter' -temporaryStoragePath 'H:\TransportRoles\data\Temp'`

## Neue Queue erstellen

1. CMD als Admin öffnen
2. Befehl eingeben `Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config `
3. Folgende Schlüssel ändern
```
<add key="QueueDatabasePath" value="<LocalPath>" />
<add key="QueueDatabaseLoggingPath" value="<LocalPath>" />
```
4. Exchange Transport Dienst durchstarten
```
net stop MSExchangeTransport 
&&
net start MSExchangeTransport 
```

## Quelle:

http://technet.microsoft.com/en-us/library/bb125177(v=exchg.150).aspx

# Exchange DAG IP ändern

## Quelle:

https://blogs.technet.microsoft.com/pfemsgil/2012/08/01/changing-dag-dag-members-ip-addresses/

# Exchange Datenbank Index "Failed and Suspended"

1. Überprüfen aller Datenbanken mit folgendem Befehl <dl><dd>`Get-MailboxDatabaseCopyStatus * | ft -auto`</dd></dl>
2. Dienste beenden <dl><dd>`stop-service MSExchangeFastSearch`</dd><dd>`stop-service HostControllerService`</dd></dl>
3. Im Datenbankpfad nach "GUID.Single" Ordnern suchen und diese löschen
4. Dienste starten <dl><dd>`start-service MSExchangeFastSearch`</dd><dd>`start-service HostControllerService`</dd></dl>

Ich habe festgestellt das es mit dieser Behebung leider nicht getan ist, bei mir hat er erst wieder richtig funktioniert nachdem ich einen Neustart durchgeführt habe.

## Quelle:

https://practical365.com/exchange-server/fix-failed-database-content-index-exchange-2013/

# Exchange Datenbank "Dirty Shutdown"

1. Exchange Powershell öffnen
2. Prüfen ob der Status zutrifft: `eseutil /mh "c:\Datenbankpfad\Defekte-Mailbox-Datenbank.edb"` unter dem Punkt **State** sollte Dirty Shutdown stehen.
3. Prüfen der dazugehörigen Logdateien: `eseutil /ml c:\Datenbank-Logdateipfad\E00`, die Logdateien sollten alle mit **OK** angezeigt werden bzw. **No damaged log files were found** am Schluß stehen um weiter fortzufahren.
4. Danach folgenden Befehl eingeben um die Datenbank wieder in einen Konsistenten zustand zu bekommen:
5. `eseutil /R E00 /l "c:\Datenbank-Logdateipfad" /s "c:\Datenbankpfad" /d "c:\Datenbankpfad"`
	/L (Case Sensitive)ist optional für eine Log Ausgabe
  Pfade:
  /l Pfad zu den Logdateien der Wiederherzustellenden Datenbank (verpflichtend)
  /s Pfad zur Datenbankdatei - gilt als **System files** Pfad (optional)
  /d Pfad zur Datenbankdatei - kein Dateiname (verpflichtend)

## Quelle:
http://blogs.technet.com/b/mspfe/archive/2012/09/06/why-exchange-databases-might-remain-dirty-after-eseutil-r-recovery.aspx

# Exchange Version anzeigen / auslesen
## Mit PowerShell
### Hauptversion anzeigen
`Get-ExchangeServer | ft Name, Edition, AdminDisplayVersion`
### Inklusive Security Updates anzeigen
```
$ExchangeServers = Get-ExchangeServer | Sort-Object Name
ForEach ($Server in $ExchangeServers) {
    Invoke-Command -ComputerName $Server.Name -ScriptBlock { Get-Command Exsetup.exe | ForEach-Object { $_.FileversionInfo } }
}
```
![exchange-detail-version.png](/media/exchange-detail-version.png)
### Quelle:
https://www.alitajran.com/find-exchange-version-with-powershell/
# Exchange Zertfikate

## Let´s Encrypt

Mit Powershell bei Let´s Encrypt Zertifikate für den Exchange Server anfordern.

### Quelle:

https://www.frankysweb.de/exchange-2016-kostenlose-zertifikate-von-lets-encrypt/

## Zertifikat auf direkt einem SMTP Connector zuweisen

```
Get-ExchangeCertificate

$cert = Get-ExchangeCertificate -Thumbprint Thumprint-des-Zertifikats

$tlscertificatename = "<i>$($cert.Issuer)<s>$($cert.Subject)"

Get-ReceiveConnector

Set-ReceiveConnector "EXServer\Client Frontend Connector" -TlsCertificateName $tlscertificatename
```

### Quelle:

https://practical365.com/exchange-server/configuring-the-tls-certificate-name-for-exchange-server-receive-connectors/

## Exchange Delegation Federation Zertifikat erneuern

1. Prüfen ob das bisherige Zertifikat abgelaufen ist: `Get-ExchangeCertificate -Thumbprint (Get-FederationTrust).OrgCertificate.Thumbprint | Format-Table -Auto Thumbprint,NotAfter`
2. Falls das Zertifikat abgelaufen ist sollte man in die Quelle des Eintrags schauen
3. Falls nicht ein neues ausstellen: `$SKI = [System.Guid]::NewGuid().ToString("N"); New-ExchangeCertificate -DomainName 'Federation' -FriendlyName "Exchange Delegation Federation" -Services Federation -SubjectKeyIdentifier $SKI -PrivateKeyExportable $true`
4. Den Fingerabdruck des Zertifikats kopieren für den nächsten Schritt.
5. Das neue Zertifikat als federation Zertifikat setzen: `Set-FederationTrust -Identity "Microsoft Federation Gateway" -Thumbprint <Fingerabdruck ohne Klammern> -RefreshMetaData`
6. Federation Proof DNS Eintrag ändern: 
6.1. Auslesen: `Get-FederatedDomainProof -DomainName <Domäne> | Format-List Thumbprint,Proof`
6.2. Neuen Hashwert kopieren (der oben angezeigte Proof Wert, unten wird der alte angezeigt) und im öffentlichen DNS pro Verbundsdomäne setzen
7. Falls es mehrere Exchange Server gibt mit folgendem Befehl überprüfen ob das Zertifikat verteilt wurde: `$Servers = Get-ExchangeServer; $Servers | foreach {Get-ExchangeCertificate -Server $_ | Where {$_.Services -match 'Federation'}} | Format-List Identity,Thumbprint,Services,Subject`
8. Das neue Zertifikat aktivieren: `Set-FederationTrust -Identity "Microsoft Federation Gateway" -PublishFederationCertificate`
9. Prüfen ob es funktioniert hat:
9.1 `Get-FederationTrust | Format-List *priv*`
9.2 `Test-FederationTrust -UserIdentity <E-Mail Adresse aus der eigenen Organisation>`

### Quelle:

https://docs.microsoft.com/en-us/exchange/renew-the-federation-certificate-exchange-2013-help

# Client Zugriff überprüfen

Mit dem im Link angegebenen Script kann überprüft werden welcher Client auf den Exchange zugreift.

## Quelle:

http://mikefrobbins.com/2014/09/18/use-powershell-to-determine-what-outlook-client-versions-are-accessing-your-exchange-servers/

# Wiederherstellungsdatenbank (Recovery DB)

## Quelle:

https://www.fpweb.net/blog/creating-a-recovery-database-in-microsoft-exchange-2010/
http://www.expta.com/2009/10/how-to-use-recovery-database-in.html

# Exchange OAB

[exchange-offlineadressbuch](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-offlineadressbuch)

## Outlook 2016 kann das Offline Adressbuch nicht aktualisieren

Wie bei dem Herren aus der Quelle wurde nach einer Migration auf Exchange 2016 und der Umstellung auf MAPI over HTTP bei Outlook Clients >2016 die Offline Adressliste nicht mehr aktualisiert.  
Falls man es händisch über **Ordner aktualisieren** versucht kommt folgender generischer Fehler **0x8004010F**.  
Beim Auslesen der Autodiscover Config konnte man sehen das keine OAB URL angezeigt wird und Outlook deshalb nicht weiß wo es das Offline Adressbuch herunterladen soll.  
Die Lösung war das Offline Adressbuch über Web zu veröffentlichen. Bisher ist mir nie aufgefallen das diese Einstellung bei einem Exchange 2016 deaktiviert ist, evtl. war die Konfiguration davor schon manuell angepasst worden, jedenfalls dubios.

1. Abfrage ob die Webveröffentlichung deaktiviert ist: `Get-OfflineAddressBook | fl name,*Distribution*`
2. Setzen der Webveröffentlichung (wenn mehrere Adressbücher vorhanden sind dann davor namentlich angeben mit *-Identity*): `Get-OfflineAddressBook | Set-OfflineAddressBock -GlobalWebDistributionEnabled $true`
3. Prüfen ob es aktiviert wurde: `Get-OfflineAddressBook | fl name,*Distribution*`

### Quelle:

https://www.stephenwagner.com/2017/11/06/offline-address-book-missing-exchange-2016/

## Global Address List

[outlook-zeigt-nicht-die-aktuelle-global-adress-list-gal-an](/de/Wiki-Seiten/Microsoft/Office/Outlook/outlook-zeigt-nicht-die-aktuelle-global-adress-list-gal-an)
