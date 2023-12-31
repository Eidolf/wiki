# Exchange 2010 Zulassen von anonymem Relay für einen Empfangsconnector

# <span class="mw-headline" id="bkmrk-beispiel%3A-1">Beispiel:</span>

## <span class="mw-headline" id="bkmrk-erstellen-des-empfan-1">Erstellen des Empfangsconnectors mithilfe der Shell</span>

In diesem Beispiel wird das Cmdlet New-ReceiveConnector verwendet, um den Empfangsconnector "Anonymous Relay" zu erstellen, der die lokale IP-Adresse 10.2.3.4 an Port 25 von einem Quellserver mit der IP-Adresse 192.168.5.77 aus überwacht:

```
New-ReceiveConnector -Name "Anonymous Relay" -Usage Custom -PermissionGroups AnonymousUsers -Bindings 10.2.3.4:25 -RemoteIpRanges 192.168.5.77
```

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-erteilen-von-relaybe-1">Erteilen von Relayberechtigungen für anonyme Verbindungen auf dem neuen Empfangsconnector mithilfe der Shell</span>

In diesem Beispiel werden die angegebenen Empfangsconnectorinformationen abgerufen, und das Ergebnis wird an das Cmdlet Add-ADPermission weitergeleitet, um für anonyme Verbindungen auf dem neuen Empfangsconnector eine Relayberechtigung einzurichten.

```
Get-ReceiveConnector "Anonymous Relay" | Add-ADPermission -User "NT AUTHORITY\ANONYMOUS LOGON" -ExtendedRights "Ms-Exch-SMTP-Accept-Any-Recipient"
```

Hinweis:  
Für Powershell Befehl muss bei einem Deutschen AD "NT-Autorität\\Anonymous-Anmeldung" für den Anonymous Benutzer auf Deutsch eingetragen werden

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://technet.microsoft.com/de-de/library/bb232021.aspx" rel="nofollow">http://technet.microsoft.com/de-de/library/bb232021.aspx</a>
```