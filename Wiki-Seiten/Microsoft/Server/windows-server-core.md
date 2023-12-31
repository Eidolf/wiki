# Windows Server Core

# <span class="mw-headline" id="bkmrk-einrichten-1">Einrichten</span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-konfigurationsmen%C3%BC-1">Konfigurationsmenü</span>

Folgenden Befehl eingeben (funktioniert in Powershell und CMD)  
`sconfig`

## <span class="mw-headline" id="bkmrk-zugriff-zulassen-1">Zugriff zulassen</span>

### <span class="mw-headline" id="bkmrk-dns-oder-hosteintrag-1">DNS oder Hosteintrag erstellen</span>

Falls der Computer in einer Workgroup ist, nicht vergessen einen Hosteintrag oder einen DNS Eintrag zu erstellen.

### <span class="mw-headline" id="bkmrk-firewallregeln-1">Firewallregeln</span>

Bei einem Core Server in einer Workgroup sollte das Remotemanagement noch aktiviert werden  
Ich habe einfachheitshalber den eingehenden Verkehr komplett zugelassen  
[Zulassen Eingehender Verbindungen für Netzwerkprofil](https://wiki.eidolf.de/index.php/Windows_Firewall#Zulassen_Eingehender_Verbindungen_f%C3%BCr_Netzwerkprofil "Windows Firewall")

### <span class="mw-headline" id="bkmrk-remote-management-1">Remote Management</span>

<div class="vector-body" id="bkmrk-server"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Server

</div></div></div>`winrm quickconfig`

<div class="vector-body" id="bkmrk-client"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Client

</div></div></div>`Set-Item wsman:\localhost\Client\TrustedHosts ServerFQDN -Concatenate -Force`

## <span class="mw-headline" id="bkmrk-quellen-1">Quellen</span>

Add Server to Servermanager:

```
https://technet.microsoft.com/en-us/library/hh831453(v=ws.11).aspx
```

Remote Management: Server 2012 R2 &amp; Hyper-V – Workgroup:

```
<a class="external free" href="https://jack-brennan.com/remotely-manage-server-2012-r2-core-hyper-v-workgroup/" rel="nofollow">https://jack-brennan.com/remotely-manage-server-2012-r2-core-hyper-v-workgroup/</a>
```