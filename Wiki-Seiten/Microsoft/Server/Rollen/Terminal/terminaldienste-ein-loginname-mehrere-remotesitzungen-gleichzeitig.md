# Terminaldienste ein Loginname mehrere Remotesitzungen gleichzeitig

Bei einem Benutzer der gleichzeit mehrer RDP Sitzungen auf einen Server ausführen will, muss entweder die Lokale Gruppenrichtlinie geändert werden oder bei einem Terminalserver unter der Terminalserverkonfiguration eine Option geändert werden.

  
Lokale Gruppenrichtlinie ( Local Group Policy Editor ):

```
Computer Configuration\Administrative Templates\Windows Components\Terminal Services\Terminal Server\Connections
```

Terminal Dienst Konfiguration ( Terminal Services Configuration ):

```
General section > Edit Settings > Restict each user to a single session
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://support.microsoft.com/kb/947723/en-us" rel="nofollow">http://support.microsoft.com/kb/947723/en-us</a>
```