---
title: Hyper-V
description: 
published: true
date: 2025-12-15T18:06:52.249Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:33:45.176Z
---

# Allgemein

## ISO Mount
[Hyper-V ISO Mount von Netzwerkfreigabe](/de/Wiki-Seiten/Microsoft/Server/Rollen/Hyper-V/hyperv-iso-mount-von-netzwerkfreigabe)

# Netzwerk

## Cluster Netzwerk
[Hyper-V Cluster Netzwerkkonfiguration](/de/Wiki-Seiten/Microsoft/Server/Rollen/Hyper-V/hyper-v-cluster-netzwerkkonfiguration)

# VLAN

Der wichtigste Hinweis zu Hyper-V in Verbindung mit VLAN´s ist, dass der Switchport als Trunk konfiguriert sein muss.  
Siehe [DELL-Switch VLAN](/de/Wiki-Seiten/Netzwerk/Hardware/DELL/dell-switch#vlan)
  
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

> WinSRV01 > Server Netzwerkkarte 2 > Hyper-V-Netzwerk VS > VLAN-ID 5 > LAG1 (Trunk / Tagged VLAN 5> SW-Port 2 Untagged VLAN 5 > Datenverkehr
{.is-info}

> WinSRV01 > Server Netzwerkkarte 1 > Hyper-V-Netzwerk VS > VLAN-ID Standard > LAG1 (Trunk / Tagged VLAN 5)> Alles Switchports außer SW-Port 2 > Datenverkehr
{.is-info}


## Quelle:

http://www.aidanfinn.com/?p=10164

# Verschachtelte / Nested Virtualisierung
Für z.B. Docker in einer virtuellen Maschine werden virtualisierungs Dienste genutzt.
Hyper-V schleift nicht standardmäßig diese Funktion an seine virtuellen Maschinen durch.
Es muss für die jeweilige Maschine ein Flag gesetzt werden.
> Voraussetzung bei der CPU muss dafür gegeben sein.
{.is-warning}

`Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true`

## Quelle:
https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/enable-nested-virtualization


# DDA (Discrete Device Assignment)

## Hinzufügen

1. `$vm = "VMName"`
2. `Set-VM $vm -GuestControlledCacheTypes $true`
3. `$location = 'PCIROOT(0)#PCI(0302)#PCI(0000)#PCI(0100)#PCI(0000)'`
(variiert nach Grafikkarte und Rechner, es kann ein Script verwendet werden
Folgender der Link zum Skript: [SurveyDDA.ps1](https://github.com/MicrosoftDocs/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)
4. `Set-VM $VM -LowMemoryMappedIoSpace 512MB` (Optional per Grafikkarte, auch über das Skript ersichtlich)
5. `Set-VM $VM -HighMemoryMappedIoSpace 1GB` (Sollte natürlich höher sein als im Punkt davor)
6. `Set-VM -Name $vm -AutomaticStopAction TurnOff`
7. `Add-VMAssignableDevice -LocationPath $Location -VMName $vm` (Grafikkarte wurde an die VM Gebunden)

## Entfernen

1. `$vm = "VMName"`
2. `$location = 'PCIROOT(0)#PCI(0302)#PCI(0000)#PCI(0100)#PCI(0000)'`
3. `Remove-VMAssignableDevice -LocationPath $location -VMName $vm`
4. `Mount-VMHostAssignableDevice -LocationPath $location`

## Quelle:

https://learn.microsoft.com/de-de/windows-server/virtualization/hyper-v/deploy/deploying-graphics-devices-using-dda
https://www.cyberciti.biz/faq/ubuntu-linux-install-nvidia-driver-latest-proprietary-driver/
(Hat nicht direkt etwas mit DDA auf Hyper-V zu tun)


# Fehler

## Dienste
[Hyper-V Hypervisor Dienst wird nach Änderung der Bootkonfiguration nicht mehr ausgeführt](/de/Wiki-Seiten/Microsoft/Server/Rollen/Hyper-V/hyperv-hypervisor-dienst-wird-nach-anderung-der-bootkonfiguration-nicht-mehr-ausgefuhrt)

## Virtual Disk

### VHDX Datei korrupt

Nach einem Stromausfall konnte ein virtueller Server nicht mehr starten, bei der Fehlerdiagnose hat sich herausgestellt das meine zweite virtuelle Datendisk angebliche korrupte Sektoren hatte und somit die Maschine nicht starten konnte. Als Bonusproblem war diese Disk auch noch mit BitLocker verschlüsselt. Ich konnte die VHDX Datei auch an keinem anderen System anhängen da es mit einer Fehlermeldung abgebrochen ist. Eine Internetrecherche hat nur empfohlen Third Party Tools zu verwenden bei den keines die Disk richtig durchsuchen konnte und weiterhin dies eh nichts gebracht hätte da die Daten verschlüsselt waren.  
Da ich aber ungern aufgebe habe ich eine Lösung die jedenfalls bei mir funktioniert hat gefunden.

1. Virtuelle Disk wenn möglich duplizieren für eine Absicherung
2. VHDXTool von folgender Seite laden [https://www.systola.com/support/KB100005/#.V48-RTUTA6M](https://www.systola.com/support/KB100005/#.V48-RTUTA6M)
3. Die vhdxtool.exe am besten neben die korrupte VHDX Datei legen um folgenden Befehl einfacher auszuführen.
4. Eine Administrative CMD öffnen und in den Pfad der beiden Dateien wechseln
5. Virtuelle Disk erweitert mit folgendem Befehl
`vhdxtool.exe extend -f data.vhdx -s 3TB`
Leider musste ich es von originalen 2,1TB auf 3TB erweitern, da alle versuche Komma Werte anzugeben fehlgeschlagen sind. Bei MB Werten hat er mehrmals gemeckert das ich die Disk größer machen muss als bisher und später das es ein multiples von X sein muss. Ich hatte zu meiner Erleichterung genügend Platz.
6. Danach konnte ich die VHDX einbinden, BitLocker mit Passwort entschlüsseln und die Dateien kopieren.

Ich gehe davon aus das der defekte Header durch die Erweiterung mit dem Tool wieder neu geschrieben wurde und sozusagen die nicht defekten Daten eine Zuordnung hatten.