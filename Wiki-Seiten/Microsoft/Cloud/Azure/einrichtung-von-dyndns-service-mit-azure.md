# Einrichtung von DynDNS Service mit Azure

# <span class="mw-headline" id="bkmrk-beschreibung%3A-1">Beschreibung:</span>

Mit einer Azure Subscription und einer gekauften Domäne kann man seinen eigenen DynDNS Dienst anbieten.  
Das zum Schluss verwendete Updatescript wurde von verschiedenen Seiten zusammenkopiert und auf eine FritzBox abgestimmt. Ehre gebührt den IT´lern aus den Quell Links die das ganze überhaupt möglich gemacht haben.

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-beispielwerte-f%C3%BCr-ei-1">Beispielwerte für Einrichtung:</span>

<div class="vector-body" id="bkmrk-dom%C3%A4ne-die-man-besit"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Domäne die man besitzt: bspazuredyn.de

</div></div></div># <span class="mw-headline" id="bkmrk-einrichtung%3A-1">Einrichtung:</span>

<div class="vector-body" id="bkmrk-in-azure-eine-dns-zo"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. In Azure eine DNS Zone Anlegen mit dem Domänennamen bspazuredyn.de  
    [![AzureDynDNS-001.png](https://wiki.eidolf.de/images/b/b4/AzureDynDNS-001.png)](https://wiki.eidolf.de/index.php/Datei:AzureDynDNS-001.png)  
    [![AzureDynDNS-002.png](https://wiki.eidolf.de/images/e/e7/AzureDynDNS-002.png)](https://wiki.eidolf.de/index.php/Datei:AzureDynDNS-002.png)
2. (Optional:) Bei der neu angelegten DNS Zone eine neue Datensatzgruppe anlegen  
    [![AzureDynDNS-003.png](https://wiki.eidolf.de/images/thumb/9/92/AzureDynDNS-003.png/700px-AzureDynDNS-003.png)](https://wiki.eidolf.de/index.php/Datei:AzureDynDNS-003.png)
3. (Optional:) Host A Eintrag manuell anlegen, wird aber eigentlich durch das Script erstellt  
    [![AzureDynDNS-004.png](https://wiki.eidolf.de/images/a/ad/AzureDynDNS-004.png)](https://wiki.eidolf.de/index.php/Datei:AzureDynDNS-004.png)
4. Jetzt kann man bei seinem Domain-Anbieter die Namensserver auf Azure als Ziel umstellen. Falls noch mehr über diese Domäne läuft als der DynDNS Dienst, sollte man vorher natürlich noch alle bestehenden Einträge bei Azure einpflegen.  
    [![AzureDynDNS-005.png](https://wiki.eidolf.de/images/7/73/AzureDynDNS-005.png)](https://wiki.eidolf.de/index.php/Datei:AzureDynDNS-005.png)
5. Service Benutzer in Azure anlegen  
    Im Quell-Link ist es ausführlicher beschrieben, doch gibt es da einen kleinen Fehler, weshalb ich die Befehle ohne Kommentar hier einfüge. 
    1. Mit Azure PowerShell verbinden (über Portal oder der normalen)
    2. `$SecureStringPassword = ConvertTo-SecureString -String "AzureServiceAccountPW" -AsPlainText -Force`
    3. `$azureAdApplication = New-AzureRmADApplication -DisplayName "powershelladminapp" -HomePage "<a class="external free" href="https://www.irgendeinewebsite.de/" rel="nofollow">https://www.irgendeinewebsite.de</a>" -IdentifierUris "<a class="external free" href="https://www.irgendeinewebsite.de/example" rel="nofollow">https://www.irgendeinewebsite.de/example</a>" -Password $SecureStringPassword`<dl><dd>Die angegebenen Website wird nirgendwo verwendet und somit kann man frei wählen.</dd></dl>
    4. `New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId`
    5. Nachfolgender Code muss unbedingt mit einem Subscription Admin ausgeführt werden, ein purer Global Admin ist nicht ausreichend. <dl><dd>`New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid`</dd></dl>
6. Nachfolgendes Script an einem Rechner als geplante Aufgabe hinterlegen der 24/7 läuft, bei mir in 5 Minuten abständen und Netzwerktechnisch Zugriff auf die Fritzbox und ins Internet hat.

</div></div></div>```
<#
    Fritzbox via TR-064 (uPNP-SOAP) steuern und auslesen 
      ------------ @colinardo / administrator.de (FritzBox)---------------
      ------------ @lewisroberts.com (Azure DynScript)---------------
      ------------ @Eidolf (Änderung / Change Script)--------------
#>
# ====== Variablen FritzBox =========
$FB_URL = "http://fritz.box"
$USERNAME = 'admin'
$PASSWORD = 'FritzPW'
# ====== Variablen Azure =========
$ZoneName = 'bspazuredyn.de'
$HostARecord = 'dyn'
$HostAddress = 'dyn.bspazuredyn.de'
$ResourceGroupName = 'DNS-Dienst'
$azureAccountName ="XXXXXXXX-bf36-4569-9884-XXXXXXXXXXXX" #Ist vom Azure Serviceaccount die ApplicationId
$azurePassword = ConvertTo-SecureString "AzureServiceAccountPW" -AsPlainText -Force
$TenantID = 'XXXXXXXX-0ac0-4ad8-9b8e-XXXXXXXXXXXX'
# ====== ENDE Variablen ====

if ($PSVersionTable.PSVersion.Major -lt 3){write-host "ERROR: Minimum Powershell Version 3.0 is required!" -F Yellow; return}

# MD5 Hashing
function md5([string]$string){
    [System.BitConverter]::ToString([System.Security.Cryptography.MD5]::Create().ComputeHash((new-object System.Text.UTF8Encoding).GetBytes($string))).replace('-','').toLower()
}

# Funktion zum senden eines SOAP Requests
function Execute-SOAPRequest {
    param(
        [Xml]$SOAPRequest,
        [string]$soapactionheader,
        [String]$URL
    )
    try{
        $wr = [System.Net.WebRequest]::Create($URL)
        $wr.Headers.Add('SOAPAction',$soapactionheader)
        $wr.ContentType = 'text/xml; charset="UTF-8"'
        $wr.Accept      = 'text/xml'
        $wr.Method      = 'POST'

        $requestStream = $wr.GetRequestStream()
        $SOAPRequest.Save($requestStream)
        $requestStream.Close()
        [System.Net.HttpWebResponse]$wresp = $wr.GetResponse()
        $responseStream = $wresp.GetResponseStream()
        $responseXML = [Xml]([System.IO.StreamReader]($responseStream)).ReadToEnd()
        $responseStream.Close()
        return $responseXML
    }catch {
        if ($_.Exception.InnerException.Response){
            throw ([System.IO.StreamReader]($_.Exception.InnerException.Response.GetResponseStream())).ReadToEnd()
        }else{
            throw $_.Exception.InnerException
        }
    }
}

# XML Service-Beschreibungs XML abrufen und Namespace setzen
[xml]$serviceinfo = Invoke-RestMethod -Method GET -Uri "$($FB_URL):49000/tr64desc.xml"
[System.Xml.XmlNamespaceManager]$ns = new-Object System.Xml.XmlNamespaceManager $serviceinfo.NameTable
$ns.AddNamespace("ns",$serviceinfo.DocumentElement.NamespaceURI)

# Request XML vorbereiten
[xml]$script:request = @"
<?xml version="1.0"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <s:Header>
        <h:ClientAuth xmlns:h="http://soap-authentication.org/digest/2001/10/" s:mustUnderstand="1">
            <Nonce></Nonce>
            <Auth></Auth>
            <UserID></UserID>
            <Realm></Realm>
        </h:ClientAuth>
    </s:Header>
    <s:Body>
    </s:Body>
</s:Envelope>
"@

# Setzt die Authentifizierungsinfos in die XML-Response
function SetAuthInfo([xml]$data){
    # Benötigte Daten für die Authentifierung aus der Response auslesen und in den neuen Request schreiben
    if ($data.Envelope.Header.NextChallenge.Nonce){
        $NONCE = $data.Envelope.Header.NextChallenge.Nonce
        $REALM = $data.Envelope.Header.NextChallenge.Realm
    }else{
        $NONCE = $data.Envelope.Header.Challenge.Nonce
        $REALM = $data.Envelope.Header.Challenge.Realm
    }
    $script:request.Envelope.Header.ClientAuth.Nonce = $NONCE
    $script:request.Envelope.Header.ClientAuth.Realm = $REALM
    $script:request.Envelope.Header.ClientAuth.UserID = $username
    $script:request.Envelope.Header.ClientAuth.Auth = md5 "$(md5 "$($username):$($REALM):$($password)"):$($NONCE)"
}

# ----------- Fritz!Box-Funktionen --------------------------
# Fritzbox Reboot
function Invoke-FBReboot(){
    $URN = 'urn:dslforum-org:service:DeviceConfig:1'
    $action = 'Reboot'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}
    
    # passenden Body Request-Tag ins XML einfügen
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($script:request.CreateElement('u',$action,$service.serviceType)) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
   
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.Envelope.Body.InnerText
}
# Basisinformationen und letzte Logeinträge abrufen
function Get-FBInfo(){
    $URN = 'urn:dslforum-org:service:DeviceInfo:1'
    $action = 'GetInfo'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($script:request.CreateElement('u',$action,$service.serviceType)) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.Envelope.Body.GetInfoResponse
}
<# Anruferliste abrufen
function Get-FBCallList(){
    param(
        [int]$maxentries = 999,
        [int]$days = 999
    )
    $URN = 'urn:dslforum-org:service:X_AVM-DE_OnTel:1'
    $action = 'GetCallList'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($script:request.CreateElement('u',$action,$service.serviceType)) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    [xml]$list = irm -uri "$($resp.Envelope.Body.GetCallListResponse.NewCallListURL)&max=$maxentries&days=$days" -Method Get
    return $list.root.call
}#>
# WAN-PPP-Info auslesen
function Get-FBWANPPPInfo(){
    $URN = 'urn:dslforum-org:service:WANPPPConnection:1'
    $action = 'GetInfo'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($script:request.CreateElement('u',$action,$service.serviceType)) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.Envelope.Body.GetInfoResponse
}
<# Aktuelle DSL-Verbindungsdaten auslesen
function Get-FBWANDSLInterfaceConfig(){
    $URN = 'urn:dslforum-org:service:WANDSLInterfaceConfig:1'
    $action = 'GetInfo'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($script:request.CreateElement('u',$action,$service.serviceType)) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.Envelope.body.GetInfoResponse
}#>
<# DSL Leitungsstatistik abfragen
function Get-FBWANDSLInterfaceStatistics(){
    $URN = 'urn:dslforum-org:service:WANDSLInterfaceConfig:1'
    $action = 'GetStatisticsTotal'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($script:request.CreateElement('u',$action,$service.serviceType)) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.Envelope.body.GetStatisticsTotalResponse
}#>
<# Portfreigaben abrufen
function Get-FBPortMappings(){
    $URN = 'urn:dslforum-org:service:WANPPPConnection:1'
    $action = 'GetGenericPortMappingEntry'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}
    
    # passenden Body Request-Tag ins XML einfügen
    $actiontag = $script:request.CreateElement('u',$action,$service.serviceType)
    $parametertag = $script:request.CreateElement('u','NewPortMappingIndex',$service.serviceType)
    $actiontag.AppendChild($parametertag) | out-null
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($actiontag) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
        
    $cnt = 0
    while($true){
        # Parameter Index für die Portregel im XML setzen
        $parametertag.InnerText = $cnt
        # Authentifizierungsdaten setzen
        SetAuthInfo $resp
        try{
            # Request senden
            $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
            # Mapping ausgeben
            $resp.Envelope.body.GetGenericPortMappingEntryResponse
            $cnt++
        }catch{
            break
        }
    }

}#>
<# DSL-Verbindung trennen
function Invoke-DSLDisconnect(){
    $URN = 'urn:dslforum-org:service:WANPPPConnection:1'
    $action = 'ForceTermination'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $actiontag = $script:request.GetElementsByTagName('s:Body')[0].AppendChild($script:request.CreateElement('u',$action,$service.serviceType))

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.Envelope.body
}#>
<# Portweiterleitungen anlegen / verändern
function Add-FBPortMapping(){
param(
    [string]$RemoteHost = '0.0.0.0',
    [ValidateRange(1,65535)][uint16]$ExternalPort,
    [ValidateSet('TCP','UDP')][string]$Protocol,
    [string]$InternalClient,
    [ValidateRange(1,65535)][uint16]$InternalPort,
    [bool]$Enabled = $true,
    [string]$Description,
    [string]$LeaseDuration = "0"
)
    $URN = 'urn:dslforum-org:service:WANPPPConnection:1'
    $action = 'AddPortMapping'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $actiontag = $script:request.CreateElement('u',$action,$service.serviceType)
    $parameter_NewRemoteHost = $script:request.CreateElement('NewRemoteHost')
    $parameter_NewExternalPort = $script:request.CreateElement('NewExternalPort')
    $parameter_NewProtocol = $script:request.CreateElement('NewProtocol')
    $parameter_NewInternalPort = $script:request.CreateElement('NewInternalPort')
    $parameter_NewInternalClient = $script:request.CreateElement('NewInternalClient')
    $parameter_NewEnabled = $script:request.CreateElement('NewEnabled')
    $parameter_NewPortMappingDescription = $script:request.CreateElement('NewPortMappingDescription')
    $parameter_NewLeaseDuration = $script:request.CreateElement('NewLeaseDuration')
    $parameter_NewRemoteHost.InnerText = $RemoteHost
    $parameter_NewExternalPort.innerText = [string]$ExternalPort
    $parameter_NewProtocol.innerText = $Protocol
    $parameter_NewInternalPort.innerText = [string]$InternalPort
    $parameter_NewInternalClient.innerText = $InternalClient
    $parameter_NewEnabled.innerText = @{$true=1;$false=0}[$Enabled]
    $parameter_NewPortMappingDescription.innerText = $Description
    $parameter_NewLeaseDuration.innerText = $LeaseDuration

    $actiontag.AppendChild($parameter_NewRemoteHost) | out-null
    $actiontag.AppendChild($parameter_NewExternalPort) | out-null
    $actiontag.AppendChild($parameter_NewProtocol) | out-null
    $actiontag.AppendChild($parameter_NewInternalPort) | out-null
    $actiontag.AppendChild($parameter_NewInternalClient) | out-null
    $actiontag.AppendChild($parameter_NewEnabled) | out-null
    $actiontag.AppendChild($parameter_NewPortMappingDescription) | out-null
    $actiontag.AppendChild($parameter_NewLeaseDuration) | out-null
    
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($actiontag) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    #$script:request.OuterXml
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.envelope.body
}#>

<# Bestimmte Portweiterleitung löschen

function Delete-FBPortMapping(){
param(
    [ValidateRange(1,65535)][uint16]$ExternalPort,
    [ValidateSet('TCP','UDP')][string]$Protocol
)
    $URN = 'urn:dslforum-org:service:WANPPPConnection:1'
    $action = 'DeletePortMapping'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $actiontag = $script:request.CreateElement('u',$action,$service.serviceType)
    $parameter_NewRemoteHost = $script:request.CreateElement('NewRemoteHost')
    $parameter_NewExternalPort = $script:request.CreateElement('NewExternalPort')
    $parameter_NewProtocol = $script:request.CreateElement('NewProtocol')
    $parameter_NewRemoteHost.InnerText = '0.0.0.0'
    $parameter_NewExternalPort.innerText = [string]$ExternalPort
    $parameter_NewProtocol.innerText = $Protocol

    $actiontag.AppendChild($parameter_NewRemoteHost) | out-null
    $actiontag.AppendChild($parameter_NewExternalPort) | out-null
    $actiontag.AppendChild($parameter_NewProtocol) | out-null
    
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($actiontag) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.envelope.body
}#>

<# Rufnummer mit der Wählhilfe wählen
function Dial-FBNumber {
param(
    [Parameter(Mandatory=$true)][ValidateNotNullOrEmpty()][string]$number
)
    $URN = 'urn:dslforum-org:service:X_VoIP:1'
    $action = 'X_AVM-DE_DialNumber'

    # Service auslesen
    $service = $serviceinfo.SelectNodes('//ns:service',$ns) | ?{$_.ServiceType -eq $URN}

    # passenden Body Request-Tag ins XML einfügen
    $actiontag = $script:request.CreateElement('u',$action,$service.serviceType)
    # Tag für die Telefonnummer erzeugen
    $parameter_number = $script:request.CreateElement('NewX_AVM-DE_PhoneNumber')
    $parameter_number.InnerText = $number
    $actiontag.AppendChild($parameter_number) | out-null
    # Actiontag zum Request hinzufügen
    $script:request.GetElementsByTagName('s:Body')[0].AppendChild($actiontag) | out-null

    # Initialer Request
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    # Authentifizierungsdaten setzen
    SetAuthInfo $resp
    # Request mit Authentifizierung abschicken
    $resp = Execute-SOAPRequest $script:request "$($service.serviceType)#$($action)" "$($FB_URL):49000$($service.controlURL)"
    return $resp.Envelope.Body
}#>

<#Function Get-DDWRTExternalIP {
    $Page = Invoke-WebRequest -UseBasicParsing -Uri "http://192.168.0.2/Status_Internet.live.asp" -Credential (Import-Clixml $ScriptPath"\DDWRTCreds.xml")
    
    $WANregex='(?<Address>{wan_ipaddr::(\b(([01]?\d?\d|2[0-4]\d|25[0-5])\.){3}([01]?\d?\d|2[0-4]\d|25[0-5])\b)})'
    $IPregex='(?<Address>(\b(([01]?\d?\d|2[0-4]\d|25[0-5])\.){3}([01]?\d?\d|2[0-4]\d|25[0-5])\b))'

    If ($Page.Content -match $WANregex) {$WANAddress = $Matches.Address}
    Else {Return $false}

    If ($WANAddress -match $IPregex) {Return $Matches.Address}
    Else {Return $false}
}#>

# Get the invocation path of the current file.
$ScriptPath = Split-Path $Script:MyInvocation.MyCommand.Path

# Set the expected IP Address. Obtain this from a DNS query or set it statically.
$EI = [System.Net.Dns]::GetHostAddresses($HostAddress) | Select-Object -ExpandProperty IPAddressToString

# Obtain the IP Address of the Internet connection.
$IP = Get-FBWANPPPInfo | select -ExpandProperty NewExternalIPAddress #Original: Get-DDWRTExternalIP

# If the IP isn't what you expected...
If ($IP -ne $EI) {

    # Login to Azure
    #$Creds = Import-Clixml -Path $ScriptPath"\AzureRMCreds.xml"
    #Login-AzureRmAccount -Credential $Creds 
    $psCred = New-Object System.Management.Automation.PSCredential($azureAccountName, $azurePassword) 
    #Login-AzureRmAccount -C -Credential $psCred
    
    Add-AzureRmAccount -Credential $psCred -TenantId $TenantID -ServicePrincipal 
    Get-AzureRmLog -StartTime (Get-Date).AddMinutes(-10) 
        
    # Update the apex record
    $RecordSet = New-AzureRmDnsRecordSet -Name $HostARecord -RecordType A -ZoneName $ZoneName -ResourceGroupName $ResourceGroupName -Ttl 60 -Overwrite -Confirm:$false
    Add-AzureRmDnsRecordConfig -RecordSet $RecordSet -Ipv4Address $IP
    Set-AzureRmDnsRecordSet -RecordSet $RecordSet

}
Else {
    Write-Output "Dynamic address ($IP) and DNS address ($EI) match."
}
```

# <span class="mw-headline" id="bkmrk-nachfolgearbeiten%3A-1">Nachfolgearbeiten:</span>

Das Passwort des Azure Serviceaccounts (App Service) läuft nach einer Zeit ab und muss erneuert werden. Im Script als **$azurePassword** angegeben  
Am einfachsten geht das über die GUI von Azure.

<div class="vector-body" id="bkmrk-azure-active-directo"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Azure Active Directory &gt; App-Registrierungen &gt; Service Benutzer auswählen <dl><dd>[![Azure-DynDNS-001.png](https://wiki.eidolf.de/images/thumb/1/1e/Azure-DynDNS-001.png/800px-Azure-DynDNS-001.png)](https://wiki.eidolf.de/index.php/Datei:Azure-DynDNS-001.png)</dd></dl>
2. Unter Zertifikate &amp; Geheimnisse &gt; einen neuen geheimen Clientschlüssel erstellen <dl><dd>[![Azure-DynDNS-002.png](https://wiki.eidolf.de/images/thumb/c/cb/Azure-DynDNS-002.png/800px-Azure-DynDNS-002.png)](https://wiki.eidolf.de/index.php/Datei:Azure-DynDNS-002.png)</dd></dl>
3. Das neue Passwort ist der erstellte Wert, der nur nach der Erstellung vollständig angezeigt wird. Deshalb wegkopieren oder am besten sofort in das verwendete Script einfügen. Danach wird der Wert nur noch teilweise angezeigt, siehe Screenshot.
4. Im Screenshot nicht ersichtlich, aber eigentlich nötig. Den alten obsoleten Schlüssel löschen.

</div></div></div># <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

Idee dazu aus folgendem Link

```
<a class="external free" href="http://www.lewisroberts.com/2016/05/28/using-azure-dns-dynamic-dns/" rel="nofollow">http://www.lewisroberts.com/2016/05/28/using-azure-dns-dynamic-dns/</a>
```

Fritzbox Abfrage in folgendem Link

```
<a class="external free" href="https://www.administrator.de/wissen/powershell-fritzbox-tr-064-netzwerk-konfigurieren-auslesen-303474.html" rel="nofollow">https://www.administrator.de/wissen/powershell-fritzbox-tr-064-netzwerk-konfigurieren-auslesen-303474.html</a>
```

Service Benutzer für Azure Dienste

```
<a class="external free" href="https://cmatskas.com/automate-login-for-azure-powershell-scripts/" rel="nofollow">https://cmatskas.com/automate-login-for-azure-powershell-scripts/</a>
<a class="external free" href="https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal" rel="nofollow">https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal</a>
```