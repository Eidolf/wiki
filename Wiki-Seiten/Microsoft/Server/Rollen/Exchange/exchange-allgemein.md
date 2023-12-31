# Exchange Allgemein

## <span class="mw-headline" id="bkmrk-anonymous-relay-1">Anonymous Relay</span>

### <span class="mw-headline" id="bkmrk-aktivieren-1">Aktivieren</span>

```
New-ReceiveConnector -Name "Anonymous Relay" -Usage Custom -PermissionGroups AnonymousUsers -Bindings ServerIP:25 -RemoteIPRanges AnonymerClient
```

```
Get-ReceiveConnector "Anonymous Relay" | Add-ADPermission -User "NT AUTHORITY\ANONYMOUS LOGON" -ExtendedRights "Ms-Exch-SMTP-Accept-Any-Recipient"
```

Bei einem deutschen AD muss der Benutzer gegen **NT-AUTORITÄT\\ANONYMOUS-ANMELDUNG** ersetzt werden.

### <span class="mw-headline" id="bkmrk-abfragen-1">Abfragen</span>

Allgemein Anonym:

```
Get-ReceiveConnector | Get-ADPermission | where {$_.User -like '*anonymous*'} | ft identity,user,extendedrights,accessrights
```

Eingrenzen auf Any Recipient:

```
Get-ReceiveConnector | Get-ADPermission | where {$_.User -like '*anonymous*' -and $_.ExtendedRights -like 'Ms-Exch-SMTP-Accept-Any-Recipient'} | ft identity,user,extendedrights,accessrights
```

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-exchange-2010-%28%C3%A4lter-1">Exchange 2010 (älterer Wiki Eintrag)</span>

[Exchange 2010 Zulassen von anonymem Relay für einen Empfangsconnector](https://wiki.eidolf.de/index.php/Exchange_2010_Zulassen_von_anonymem_Relay_f%C3%BCr_einen_Empfangsconnector "Exchange 2010 Zulassen von anonymem Relay für einen Empfangsconnector")

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-datenbanken-l%C3%B6schen-1">Datenbanken löschen</span>

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

## <span class="mw-headline" id="bkmrk-impersonation-rechte-1">Impersonation Rechte</span>

Erweiterte AD Berechtigung um einem Dienstbenutzer Berechtigung auf Postfächer in deren Namen zu erteilen.

### <span class="mw-headline" id="bkmrk-anwendungsbeispiele-1">Anwendungsbeispiele</span>

#### <span class="mw-headline" id="bkmrk-berechtigung-eines-s-1">Berechtigung eines Service Accounts auf Shared Mailboxen in AD Gruppe</span>

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

## <span class="mw-headline" id="bkmrk-exchange-ein-neustar-1">Exchange ein Neustart ist ausstehend</span>

Folgenden Regeintrag löschen:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\PendingFileRenameOperations
```

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://technet.microsoft.com/de-de/library/cc164360%28v=exchg.80%29.aspx" rel="nofollow">https://technet.microsoft.com/de-de/library/cc164360%28v=exchg.80%29.aspx</a>
```

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-exchange-empfangs-co-1">Exchange Empfangs Connector EHLO / HELO Server ändern</span>

[Exchange HELO Begrüßung ändern](https://wiki.eidolf.de/index.php/Exchange_HELO_Begr%C3%BC%C3%9Fung_%C3%A4ndern "Exchange HELO Begrüßung ändern")

## <span id="bkmrk--3"></span><span class="mw-headline" id="bkmrk-exchange-queue-pfad--1">Exchange Queue Pfad ändern</span>

### <span class="mw-headline" id="bkmrk-bestehende-queue-ver-1">Bestehende Queue verschieben</span>

Auch für eine bestehende Queue kann man die unter **Neue Queue erstellen** beschriebene Version verwenden, doch gibt es ein Script das alle Aufgaben automatisch erledigt.

1. Eine Administrative Exchange PowerShell öffnen.
2. `cd $ExScripts`
3. Im Falle man möchte die gesamte Queue auf das **Laufwerk H:** verschieben verwendet man folgenden Code.  
    Auf dem Laufwerk H: müssen **keine Ordner** angelegt sein, diese werden durch das Script erstellt und berechtigt. 
    1. `.\Move-TransportDatabase.ps1 -queueDatabasePath 'H:\TransportRoles\data\Queue' -queueDatabaseLoggingPath 'H:\TransportRoles\data\Queue\Logs' -iPFilterDatabasePath 'H:\TransportRoles\data\IpFilter' -iPFilterDatabaseLoggingPath 'H:\TransportRoles\data\IpFilter' -temporaryStoragePath 'H:\TransportRoles\data\Temp'`

### <span class="mw-headline" id="bkmrk-neue-queue-erstellen-1">Neue Queue erstellen</span>

1. CMD als Admin öffnen
2. Befehl eingeben ```
     Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config 
    ```
3. Folgende Schlüssel ändern ```
     <add key="QueueDatabasePath" value="<LocalPath>" />
    ```
    
    ```
     <add key="QueueDatabaseLoggingPath" value="<LocalPath>" />
    ```
4. Exchange Transport Dienst durchstarten ```
     net stop MSExchangeTransport 
    ```
    
    ```
     net start MSExchangeTransport 
    ```

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/bb125177(v=exchg.150).aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/bb125177(v=exchg.150).aspx</a>
```

## <span id="bkmrk--4"></span><span class="mw-headline" id="bkmrk-exchange-dag-ip-%C3%A4nde-1">Exchange DAG IP ändern</span>

### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://blogs.technet.microsoft.com/pfemsgil/2012/08/01/changing-dag-dag-members-ip-addresses/" rel="nofollow">https://blogs.technet.microsoft.com/pfemsgil/2012/08/01/changing-dag-dag-members-ip-addresses/</a>
```

## <span id="bkmrk--5"></span><span class="mw-headline" id="bkmrk-exchange-datenbank-i-1">Exchange Datenbank Index "Failed and Suspended"</span>

1. Überprüfen aller Datenbanken mit folgendem Befehl <dl><dd>`Get-MailboxDatabaseCopyStatus * | ft -auto`</dd></dl>
2. Dienste beenden <dl><dd>`stop-service MSExchangeFastSearch`</dd><dd>`stop-service HostControllerService`</dd></dl>
3. Im Datenbankpfad nach "GUID.Single" Ordnern suchen und diese löschen
4. Dienste starten <dl><dd>`start-service MSExchangeFastSearch`</dd><dd>`start-service HostControllerService`</dd></dl>

Ich habe festgestellt das es mit dieser Behebung leider nicht getan ist, bei mir hat er erst wieder richtig funktioniert nachdem ich einen Neustart durchgeführt habe.

### <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="https://practical365.com/exchange-server/fix-failed-database-content-index-exchange-2013/" rel="nofollow">https://practical365.com/exchange-server/fix-failed-database-content-index-exchange-2013/</a>
```

## <span id="bkmrk--6"></span><span class="mw-headline" id="bkmrk-exchange-datenbank-%22-1">Exchange Datenbank "Dirty Shutdown"</span>

1. Exchange Powershell öffnen
2. Prüfen ob der Status zutrifft: `eseutil /mh "c:\Datenbankpfad\Defekte-Mailbox-Datenbank.edb"` unter dem Punkt **State** sollte Dirty Shutdown stehen.
3. Prüfen der dazugehörigen Logdateien: `eseutil /ml c:\Datenbank-Logdateipfad\E00`, die Logdateien sollten alle mit **OK** angezeigt werden bzw. **No damaged log files were found** am Schluß stehen um weiter fortzufahren.
4. Danach folgenden Befehl eingeben um die Datenbank wieder in einen Konsistenten zustand zu bekommen:
5. `eseutil /R E00 /l "c:\Datenbank-Logdateipfad" /s "c:\Datenbankpfad" /d "c:\Datenbankpfad"`<dl><dd>/L (Case Sensitive)ist optional für eine Log Ausgabe</dd><dd>Pfade:</dd><dd>/l Pfad zu den Logdateien der Wiederherzustellenden Datenbank (verpflichtend)</dd><dd>/s Pfad zur Datenbankdatei - gilt als **System files** Pfad (optional)</dd><dd>/d Pfad zur Datenbankdatei - kein Dateiname (verpflichtend)</dd></dl>

### <span class="mw-headline" id="bkmrk-quelle%3A-9">Quelle:</span>

```
<a class="external free" href="http://blogs.technet.com/b/mspfe/archive/2012/09/06/why-exchange-databases-might-remain-dirty-after-eseutil-r-recovery.aspx" rel="nofollow">http://blogs.technet.com/b/mspfe/archive/2012/09/06/why-exchange-databases-might-remain-dirty-after-eseutil-r-recovery.aspx</a>
```

## <span class="mw-headline" id="bkmrk-exchange-zertfikate-1">Exchange Zertfikate</span>

### <span id="bkmrk--7"></span><span class="mw-headline" id="bkmrk-let%C2%B4s-encrypt-1">Let´s Encrypt</span>

Mit Powershell bei Let´s Encrypt Zertifikate für den Exchange Server anfordern.

#### <span class="mw-headline" id="bkmrk-quelle%3A-11">Quelle:</span>

```
<a class="external free" href="https://www.frankysweb.de/exchange-2016-kostenlose-zertifikate-von-lets-encrypt/" rel="nofollow">https://www.frankysweb.de/exchange-2016-kostenlose-zertifikate-von-lets-encrypt/</a>
```

### <span class="mw-headline" id="bkmrk-zertifikat-auf-direk-1">Zertifikat auf direkt einem SMTP Connector zuweisen</span>

```
Get-ExchangeCertificate

$cert = Get-ExchangeCertificate -Thumbprint Thumprint-des-Zertifikats

$tlscertificatename = "<i>$($cert.Issuer)<s>$($cert.Subject)"

Get-ReceiveConnector

Set-ReceiveConnector "EXServer\Client Frontend Connector" -TlsCertificateName $tlscertificatename
```

#### <span class="mw-headline" id="bkmrk-quelle%3A-13">Quelle:</span>

```
<a class="external free" href="https://practical365.com/exchange-server/configuring-the-tls-certificate-name-for-exchange-server-receive-connectors/" rel="nofollow">https://practical365.com/exchange-server/configuring-the-tls-certificate-name-for-exchange-server-receive-connectors/</a>
```

### <span class="mw-headline" id="bkmrk-exchange-delegation--1">Exchange Delegation Federation Zertifikat erneuern</span>

1. Prüfen ob das bisherige Zertifikat abgelaufen ist: `Get-ExchangeCertificate -Thumbprint (Get-FederationTrust).OrgCertificate.Thumbprint | Format-Table -Auto Thumbprint,NotAfter`
2. Falls das Zertifikat abgelaufen ist sollte man in die Quelle des Eintrags schauen
3. Falls nicht ein neues ausstellen: `$SKI = [System.Guid]::NewGuid().ToString("N"); New-ExchangeCertificate -DomainName 'Federation' -FriendlyName "Exchange Delegation Federation" -Services Federation -SubjectKeyIdentifier $SKI -PrivateKeyExportable $true`
4. Den Fingerabdruck des Zertifikats kopieren für den nächsten Schritt.
5. Das neue Zertifikat als federation Zertifikat setzen: `Set-FederationTrust -Identity "Microsoft Federation Gateway" -Thumbprint <Fingerabdruck ohne Klammern> -RefreshMetaData`
6. Federation Proof DNS Eintrag ändern: 
    1. Auslesen: `Get-FederatedDomainProof -DomainName <Domäne> | Format-List Thumbprint,Proof`
    2. Neuen Hashwert kopieren (der oben angezeigte Proof Wert, unten wird der alte angezeigt) und im öffentlichen DNS pro Verbundsdomäne setzen
7. Falls es mehrere Exchange Server gibt mit folgendem Befehl überprüfen ob das Zertifikat verteilt wurde: `$Servers = Get-ExchangeServer; $Servers | foreach {Get-ExchangeCertificate -Server $_ | Where {$_.Services -match 'Federation'}} | Format-List Identity,Thumbprint,Services,Subject`
8. Das neue Zertifikat aktivieren: `Set-FederationTrust -Identity "Microsoft Federation Gateway" -PublishFederationCertificate`
9. Prüfen ob es funktioniert hat: 
    1. `Get-FederationTrust | Format-List *priv*`
    2. `Test-FederationTrust -UserIdentity <E-Mail Adresse aus der eigenen Organisation>`

#### <span class="mw-headline" id="bkmrk-quelle%3A-15">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/en-us/exchange/renew-the-federation-certificate-exchange-2013-help" rel="nofollow">https://docs.microsoft.com/en-us/exchange/renew-the-federation-certificate-exchange-2013-help</a>
```

## <span id="bkmrk--8"></span><span class="mw-headline" id="bkmrk-client-zugriff-%C3%BCberp-1">Client Zugriff überprüfen</span>

Mit dem im Link angegebenen Script kann überprüft werden welcher Client auf den Exchange zugreift.

### <span class="mw-headline" id="bkmrk-quelle%3A-17">Quelle:</span>

```
<a class="external free" href="http://mikefrobbins.com/2014/09/18/use-powershell-to-determine-what-outlook-client-versions-are-accessing-your-exchange-servers/" rel="nofollow">http://mikefrobbins.com/2014/09/18/use-powershell-to-determine-what-outlook-client-versions-are-accessing-your-exchange-servers/</a>
```

## <span id="bkmrk--9"></span><span class="mw-headline" id="bkmrk-wiederherstellungsda-1">Wiederherstellungsdatenbank (Recovery DB)</span>

### <span class="mw-headline" id="bkmrk-quelle%3A-19">Quelle:</span>

```
<a class="external free" href="https://www.fpweb.net/blog/creating-a-recovery-database-in-microsoft-exchange-2010/" rel="nofollow">https://www.fpweb.net/blog/creating-a-recovery-database-in-microsoft-exchange-2010/</a>
<a class="external free" href="http://www.expta.com/2009/10/how-to-use-recovery-database-in.html" rel="nofollow">http://www.expta.com/2009/10/how-to-use-recovery-database-in.html</a>
```

## <span class="mw-headline" id="bkmrk-exchange-oab-1">Exchange OAB</span>

[Exchange Offlineadressbuch](https://wiki.eidolf.de/index.php/Exchange_Offlineadressbuch "Exchange Offlineadressbuch")

### <span class="mw-headline" id="bkmrk-outlook-2016-kann-da-1">Outlook 2016 kann das Offline Adressbuch nicht aktualisieren</span>

Wie bei dem Herren aus der Quelle wurde nach einer Migration auf Exchange 2016 und der Umstellung auf MAPI over HTTP bei Outlook Clients &gt;2016 die Offline Adressliste nicht mehr aktualisiert.  
Falls man es händisch über **Ordner aktualisieren** versucht kommt folgender generischer Fehler **0x8004010F**.  
Beim Auslesen der Autodiscover Config konnte man sehen das keine OAB URL angezeigt wird und Outlook deshalb nicht weiß wo es das Offline Adressbuch herunterladen soll.  
Die Lösung war das Offline Adressbuch über Web zu veröffentlichen. Bisher ist mir nie aufgefallen das diese Einstellung bei einem Exchange 2016 deaktiviert ist, evtl. war die Konfiguration davor schon manuell angepasst worden, jedenfalls dubios.

1. Abfrage ob die Webveröffentlichung deaktiviert ist: `Get-OfflineAddressBook | fl name,*Distribution*`
2. Setzen der Webveröffentlichung (wenn mehrere Adressbücher vorhanden sind dann davor namentlich angeben mit *-Identity*): `Get-OfflineAddressBook | Set-OfflineAddressBock -GlobalWebDistributionEnabled $true`
3. Prüfen ob es aktiviert wurde: `Get-OfflineAddressBook | fl name,*Distribution*`

#### <span class="mw-headline" id="bkmrk-quelle%3A-21">Quelle:</span>

```
<a class="external free" href="https://www.stephenwagner.com/2017/11/06/offline-address-book-missing-exchange-2016/" rel="nofollow">https://www.stephenwagner.com/2017/11/06/offline-address-book-missing-exchange-2016/</a>
```

### <span class="mw-headline" id="bkmrk-global-address-list-1">Global Address List</span>

[Outlook zeigt nicht die aktuelle Global Adress List (GAL) an](https://wiki.eidolf.de/index.php/Outlook_zeigt_nicht_die_aktuelle_Global_Adress_List_(GAL)_an "Outlook zeigt nicht die aktuelle Global Adress List (GAL) an")