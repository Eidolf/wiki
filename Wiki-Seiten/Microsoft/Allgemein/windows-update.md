---
title: Windows Update
description: 
published: true
date: 2026-08-31T09:46:03.954Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:29:31.735Z
---

# Fehler</span>

## MS Updates werden nicht richtig installiert

Mit diesem Tool wird MS Update auf inkonsistenz überprüft.  
  
Quelle:

http://support.microsoft.com/kb/947821

## Windows 7 und Windows 2008 R2 können Updates nicht mehr installieren ab einem gewissen Zeitpunkt

Falls die genannten Betriebssysteme längere Zeit keine Updates bekommen haben und sozusagen noch bevor Updates auf Rollups umgestellt wurden der letzte Patchstand war, kann es sein das keine Updates nach August 2019 installiert werden können.  
Dies hat mit der Umstellung von Patchpaketen mit SHA1 Kodierung auf SHA2 Kodierung zu tun.  
Es gab dafür einen Patch im März 2019 der aber mit Rollups scheinbar ab und an nicht mitinstalliert wird.

### Genaue Fehlerbeschreibung:

Windows Updates **KB4512506** oder **KB4512486** verursachen Error **0x80092004**  
Alle nachfolgenden Rollups haben genau den gleichen Fehler im DISM Log oder im Windows Update Log.

### Lösung:

Installieren des Service Stack Updates **KB4490628**  
[https://www.catalog.update.microsoft.com/Search.aspx?q=KB4490628](https://www.catalog.update.microsoft.com/Search.aspx?q=KB4490628)

### Quelle:
https://www.borncity.com/blog/2019/08/14/windows-updates-kb4512506-kb4512486-verursachen-error-0x80092004/

# Überprüfen

## Mit DISM KB Artikel suchen

`dism /online /get-packages | findstr KB2894856`

## Update Einstellungen über Registry

Pfad zu den Einstellungen:  
`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate`  
Folgende Einstellungen können z.B. gesetzt werden.

- "WUServer"="https://WSUS-Server.domain.de:8531"
- "WUStatusServer"="https://WSUS-Server.domain.de:8531"
- "UpdateServiceUrlAlternate"=""
- "ElevateNonAdmins"=dword:00000001
- "TargetGroupEnabled"=dword:00000001
- "TargetGroup"="Server-Patch-Tuesday"

Pfad zu weiteren Einstellungen:  
`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`  
Folgende Einstellungen können z.B. gesetzt werden.

- "UseWUServer"=dword:00000001
- "NoAutoRebootWithLoggedOnUsers"=dword:00000001
- "NoAutoUpdate"=dword:00000000
- "AUOptions"=dword:00000004
- "ScheduledInstallDay"=dword:00000006
- "ScheduledInstallTime"=dword:0000000c
- "ScheduledInstallThirdWeek"=dword:00000001

### Quelle:
https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708449(v=ws.10)