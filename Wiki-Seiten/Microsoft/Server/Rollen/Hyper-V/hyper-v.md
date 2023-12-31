# Hyper-V

# <span class="mw-headline" id="bkmrk-allgemein-1">Allgemein</span>

## <span class="mw-headline" id="bkmrk-iso-mount-1">ISO Mount</span>

[HyperV ISO Mount von Netzwerkfreigabe](https://wiki.eidolf.de/index.php/HyperV_ISO_Mount_von_Netzwerkfreigabe "HyperV ISO Mount von Netzwerkfreigabe")

# <span class="mw-headline" id="bkmrk-netzwerk-1">Netzwerk</span>

## <span class="mw-headline" id="bkmrk-cluster-netzwerk-1">Cluster Netzwerk</span>

[Hyper-V Cluster Netzwerkkonfiguration](https://wiki.eidolf.de/index.php/Hyper-V_Cluster_Netzwerkkonfiguration "Hyper-V Cluster Netzwerkkonfiguration")

# <span class="mw-headline" id="bkmrk-vlan-1">VLAN</span>

Der wichtigste Hinweis zu Hyper-V in Verbindung mit VLAN´s ist, dass der Switchport als Trunk konfiguriert sein muss.  
Siehe [DELL-Switch#VLAN](https://wiki.eidolf.de/index.php/DELL-Switch#VLAN "DELL-Switch")  
  
**Beispiel:**  
Windows Server Name = ***WinSRV01***  
Hyper-V virtueller Switch = ***Hyper-V-Netzwerk***  
Der virtuelle Switch ist '***nicht an den Host gebunden'*** und ist am Switch gebündelt ***LAG1***  
Erste Netzwerkkarte verwendet das ***Standard VLAN***  
VLAN-ID an Netzwerkarte 2 = ***5***  
Alle anderen Server verwenden das Standard VLAN am gleichen virtuellen Switch.  
LAG1 = ***Trunk und mit VLAN 5 getagged***  
Switchport 2 = ***Untagged VLAN 5***  
Alles anderen Switchports = ***Standard VLAN***

`WinSRV01 > Server Netzwerkkarte 2 > Hyper-V-Netzwerk VS > VLAN-ID 5 > LAG1 (Trunk / Tagged VLAN 5> SW-Port 2 Untagged VLAN 5 > Datenverkehr`  
`WinSRV01 > Server Netzwerkkarte 1 > Hyper-V-Netzwerk VS > VLAN-ID Standard > LAG1 (Trunk / Tagged VLAN 5)> Alles Switchports außer SW-Port 2 > Datenverkehr`

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.aidanfinn.com/?p=10164" rel="nofollow">http://www.aidanfinn.com/?p=10164</a>
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-dda-%28discrete-device-1">DDA (Discrete Device Assignment)</span>

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-hinzuf%C3%BCgen-1">Hinzufügen</span>

<div class="vector-body" id="bkmrk-%24vm-%3D-%22vmname%22-set-v"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `$vm = "VMName"`
2. `Set-VM $vm -GuestControlledCacheTypes $true`
3. `$location = 'PCIROOT(0)#PCI(0302)#PCI(0000)#PCI(0100)#PCI(0000)'` (variiert nach Grafikkarte und Rechner, es kann ein Script verwendet werden <dl><dd>Folgender der Link zum Skript: [SurveyDDA.ps1](https://github.com/MicrosoftDocs/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)</dd></dl>
4. `Set-VM $VM -LowMemoryMappedIoSpace 512MB` (Optional per Grafikkarte, auch über das Skript ersichtlich)
5. `Set-VM $VM -HighMemoryMappedIoSpace 1GB` (Sollte natürlich höher sein als im Punkt davor)
6. `Set-VM -Name $vm -AutomaticStopAction TurnOff`
7. `Add-VMAssignableDevice -LocationPath $Location -VMName $vm` (Grafikkarte wurde an die VM Gebunden)

</div></div></div>## <span class="mw-headline" id="bkmrk-entfernen-1">Entfernen</span>

<div class="vector-body" id="bkmrk-%24vm-%3D-%22vmname%22-%24loca"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `$vm = "VMName"`
2. `$location = 'PCIROOT(0)#PCI(0302)#PCI(0000)#PCI(0100)#PCI(0000)'`
3. `Remove-VMAssignableDevice -LocationPath $location -VMName $vm`
4. `Mount-VMHostAssignableDevice -LocationPath $location`

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://learn.microsoft.com/de-de/windows-server/virtualization/hyper-v/deploy/deploying-graphics-devices-using-dda" rel="nofollow">https://learn.microsoft.com/de-de/windows-server/virtualization/hyper-v/deploy/deploying-graphics-devices-using-dda</a>
<a class="external free" href="https://www.cyberciti.biz/faq/ubuntu-linux-install-nvidia-driver-latest-proprietary-driver/" rel="nofollow">https://www.cyberciti.biz/faq/ubuntu-linux-install-nvidia-driver-latest-proprietary-driver/</a> (Hat nicht direkt etwas mit DDA auf Hyper-V zu tun
```

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span class="mw-headline" id="bkmrk-dienste-1">Dienste</span>

[HyperV Hypervisor Dienst wird nach Änderung der Bootkonfiguration nicht mehr ausgeführt](https://wiki.eidolf.de/index.php/HyperV_Hypervisor_Dienst_wird_nach_%C3%84nderung_der_Bootkonfiguration_nicht_mehr_ausgef%C3%BChrt "HyperV Hypervisor Dienst wird nach Änderung der Bootkonfiguration nicht mehr ausgeführt")

## <span class="mw-headline" id="bkmrk-virtual-disk-1">Virtual Disk</span>

### <span class="mw-headline" id="bkmrk-vhdx-datei-korrupt-1">VHDX Datei korrupt</span>

Nach einem Stromausfall konnte ein virtueller Server nicht mehr starten, bei der Fehlerdiagnose hat sich herausgestellt das meine zweite virtuelle Datendisk angebliche korrupte Sektoren hatte und somit die Maschine nicht starten konnte. Als Bonusproblem war diese Disk auch noch mit BitLocker verschlüsselt. Ich konnte die VHDX Datei auch an keinem anderen System anhängen da es mit einer Fehlermeldung abgebrochen ist. Eine Internetrecherche hat nur empfohlen Third Party Tools zu verwenden bei den keines die Disk richtig durchsuchen konnte und weiterhin dies eh nichts gebracht hätte da die Daten verschlüsselt waren.  
Da ich aber ungern aufgebe habe ich eine Lösung die jedenfalls bei mir funktioniert hat gefunden.

<div class="vector-body" id="bkmrk-virtuelle-disk-wenn-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Virtuelle Disk wenn möglich duplizieren für eine Absicherung
2. VHDXTool von folgender Seite laden [https://www.systola.com/support/KB100005/#.V48-RTUTA6M](https://www.systola.com/support/KB100005/#.V48-RTUTA6M)
3. Die vhdxtool.exe am besten neben die korrupte VHDX Datei legen um folgenden Befehl einfacher auszuführen.
4. Eine Administrative CMD öffnen und in den Pfad der beiden Dateien wechseln
5. Virtuelle Disk erweitert mit folgendem Befehl <dl><dd>vhdxtool.exe extend -f data.vhdx -s 3TB</dd><dd>Leider musste ich es von originalen 2,1TB auf 3TB erweitern, da alle versuche Komma Werte anzugeben fehlgeschlagen sind. Bei MB Werten hat er mehrmals gemeckert das ich die Disk größer machen muss als bisher und später das es ein multiples von X sein muss. Ich hatte zu meiner Erleichterung genügend Platz.</dd></dl>
6. Danach konnte ich die VHDX einbinden, BitLocker mit Passwort entschlüsseln und die Dateien kopieren.

</div></div></div>Ich gehe davon aus das der defekte Header durch die Erweiterung mit dem Tool wieder neu geschrieben wurde und sozusagen die nicht defekten Daten eine Zuordnung hatten.