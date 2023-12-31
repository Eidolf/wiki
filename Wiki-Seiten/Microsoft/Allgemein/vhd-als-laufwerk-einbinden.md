# VHD als Laufwerk einbinden

# <span class="mw-headline" id="bkmrk-server-2008-1">Server 2008</span>

Bei Server 2008 R2 kann man eine VHD Datei direkt über den Festplattenmanager (Datenträgerverwaltung) einbinden mit Mausklicks. Bei Server 2008 geht das nur über ein VBS Skript.

Beispiel Mount Skript

```
Option Explicit 

Dim WMI 
Dim VHD 
Dim filename 

filename = "G:\HyperV\Windows Server 2003\Harddisk1.vhd" 

Set WMI = GetObject("winmgmts:\\.\root\virtualization") 

Set VHD= WMI.ExecQuery("SELECT * FROM Msvm_ImageManagementService").ItemIndex(0) 

VHD.Mount(filename) 
REM VHD.Unmount(filename)
```

Beispiel Dismount Skript

```
Option Explicit 

Dim WMI 
Dim VHD 
Dim filename 

filename = "G:\HyperV\Windows Server 2003\Harddisk1.vhd" 

Set WMI = GetObject("winmgmts:\\.\root\virtualization") 

Set VHD= WMI.ExecQuery("SELECT * FROM Msvm_ImageManagementService").ItemIndex(0) 

REM VHD.Mount(filename) 
VHD.Unmount(filename)
```