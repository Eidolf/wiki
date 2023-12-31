# WSUS Client 100% Auslastung

Bei XP oder 2000 Clients kann es sein das die svhost.exe (eventid 26) den Client bis zu 100% auslastet.  
Hierbei folgendermaßen vorgehen:

<div class="vector-body" id="bkmrk-installation-windows"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Installation Windows Update Agent 3.0 
    - [http://download.windowsupdate.com/v7/windowsupdate/redist/standalone/WindowsUpdateAgent30-x86.exe](http://download.windowsupdate.com/v7/windowsupdate/redist/standalone/WindowsUpdateAgent30-x86.exe)
    - [http://download.windowsupdate.com/v7/windowsupdate/redist/standalone/WindowsUpdateAgent30-x64.exe](http://download.windowsupdate.com/v7/windowsupdate/redist/standalone/WindowsUpdateAgent30-x64.exe)
    - [http://download.windowsupdate.com/v7/windowsupdate/redist/standalone/WindowsUpdateAgent30-ia64.exe](http://download.windowsupdate.com/v7/windowsupdate/redist/standalone/WindowsUpdateAgent30-ia64.exe)
2. KB927891/MSI fix installieren: 
    - [http://support.microsoft.com/kb/927891](http://support.microsoft.com/kb/927891)

</div></div></div>Eventuell lässt sich der Client nur über die Commandokonsole installieren.

```
WindowsUpdateAgent30-<platform>.exe /quiet /norestart /wuforce
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.mcseboard.de/tipps-links-5/wsus-bringt-clients-100-auslastung-svchost-eventid-26-a-4-111725.html" rel="nofollow">http://www.mcseboard.de/tipps-links-5/wsus-bringt-clients-100-auslastung-svchost-eventid-26-a-4-111725.html</a>
<a class="external free" href="http://msmvps.com/blogs/donna/archive/2007/05/15/microsoft-s-follow-up-fix-on-100-cpu-usage-svchost-exe-msi.aspx" rel="nofollow">http://msmvps.com/blogs/donna/archive/2007/05/15/microsoft-s-follow-up-fix-on-100-cpu-usage-svchost-exe-msi.aspx</a>
<a class="external free" href="http://www.experts-exchange.com/OS/Microsoft_Operating_Systems/Windows/XP/Q_24851472.html" rel="nofollow">http://www.experts-exchange.com/OS/Microsoft_Operating_Systems/Windows/XP/Q_24851472.html</a>
```