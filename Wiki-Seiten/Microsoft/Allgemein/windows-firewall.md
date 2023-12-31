# Windows Firewall

# <span class="mw-headline" id="bkmrk-powershell-1">Powershell</span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-zulassen-eingehender-1">Zulassen Eingehender Verbindungen für Netzwerkprofil</span>

`Set-NetFirewallProfile -Profile Domain,Private,Public -DefaultInboundAction Allow`

## <span class="mw-headline" id="bkmrk-quellen%3A-1">Quellen:</span>

```
<a class="external free" href="https://msdn.microsoft.com/de-de/library/hh831755(v=ws.11).aspx" rel="nofollow">https://msdn.microsoft.com/de-de/library/hh831755(v=ws.11).aspx</a>
```