# Windows Update

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span class="mw-headline" id="bkmrk-ms-updates-werden-ni-1">MS Updates werden nicht richtig installiert</span>

Mit diesem Tool wird MS Update auf inkonsistenz überprüft.  
  
Quelle:

```
<a class="external free" href="http://support.microsoft.com/kb/947821" rel="nofollow">http://support.microsoft.com/kb/947821</a>
```

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-windows-7-und-window-1">Windows 7 und Windows 2008 R2 können Updates nicht mehr installieren ab einem gewissen Zeitpunkt</span>

Falls die genannten Betriebssysteme längere Zeit keine Updates bekommen haben und sozusagen noch bevor Updates auf Rollups umgestellt wurden der letzte Patchstand war, kann es sein das keine Updates nach August 2019 installiert werden können.  
Dies hat mit der Umstellung von Patchpaketen mit SHA1 Kodierung auf SHA2 Kodierung zu tun.  
Es gab dafür einen Patch im März 2019 der aber mit Rollups scheinbar ab und an nicht mitinstalliert wird.

### <span class="mw-headline" id="bkmrk-genaue-fehlerbeschre-1">Genaue Fehlerbeschreibung:</span>

Windows Updates **KB4512506** oder **KB4512486** verursachen Error **0x80092004**  
Alle nachfolgenden Rollups haben genau den gleichen Fehler im DISM Log oder im Windows Update Log.

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

Installieren des Service Stack Updates **KB4490628**  
[https://www.catalog.update.microsoft.com/Search.aspx?q=KB4490628](https://www.catalog.update.microsoft.com/Search.aspx?q=KB4490628)

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://www.borncity.com/blog/2019/08/14/windows-updates-kb4512506-kb4512486-verursachen-error-0x80092004/" rel="nofollow">https://www.borncity.com/blog/2019/08/14/windows-updates-kb4512506-kb4512486-verursachen-error-0x80092004/</a>
```

# <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-%C3%9Cberpr%C3%BCfen-1">Überprüfen</span>

## <span class="mw-headline" id="bkmrk-mit-dism-kb-artikel--1">Mit DISM KB Artikel suchen</span>

`dism /online /get-packages | findstr KB2894856`

## <span id="bkmrk--3"></span><span class="mw-headline" id="bkmrk-update-einstellungen-1">Update Einstellungen über Registry</span>

Pfad zu den Einstellungen:  
`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate`  
Folgende Einstellungen können z.B. gesetzt werden.

<div class="vector-body" id="bkmrk-%22wuserver%22%3D%22https%3A%2F%2F"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- "WUServer"="https://WSUS-Server.domain.de:8531"
- "WUStatusServer"="https://WSUS-Server.domain.de:8531"
- "UpdateServiceUrlAlternate"=""
- "ElevateNonAdmins"=dword:00000001
- "TargetGroupEnabled"=dword:00000001
- "TargetGroup"="Server-Patch-Tuesday"

</div></div></div>Pfad zu weiteren Einstellungen:  
`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`  
Folgende Einstellungen können z.B. gesetzt werden.

<div class="vector-body" id="bkmrk-%22usewuserver%22%3Ddword%3A"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- "UseWUServer"=dword:00000001
- "NoAutoRebootWithLoggedOnUsers"=dword:00000001
- "NoAutoUpdate"=dword:00000000
- "AUOptions"=dword:00000004
- "ScheduledInstallDay"=dword:00000006
- "ScheduledInstallTime"=dword:0000000c
- "ScheduledInstallThirdWeek"=dword:00000001

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708449(v=ws.10)" rel="nofollow">https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708449(v=ws.10)</a>
```