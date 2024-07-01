---
title: windows-server-2016
description: 
published: true
date: 2024-07-01T10:54:47.047Z
tags: 
editor: markdown
dateCreated: 2023-12-31T11:55:58.473Z
---

# Windows Server 2016

# Features

## Storage Spaces

### Schreibgeschwindigkeit schlecht

Falls die Schreibgeschwindigkeit bei **Storage Spaces mit Parity** sehr schlecht sind, sollten alles Physikalischen Festplatten im Gerätemanager überprüft werden.  
Gerätemanager &gt; Laufwerke &gt; Eigenschaften der HDD &gt; Richtlinien  
Es sollte "Schreibcache auf dem Gerät aktivieren" angehakt sein. Weiterhin muss auf **!eigenes Risiko!** dem Storage Space manuell vorgegeben werden das eine USV vorhanden ist.

1. Administrative PowerShell öffnen
2. `Set-StoragePool -FriendlyName StoragePoolName-IsPowerProtected $true`

Insgesamt verbessert sich dadurch die Leistung maximal auf das doppelte, leider ist der Wert immer noch nicht gut.

### Festplatte entfernen aus Verbund

Ich konnte eine Festplatte ohne Probleme über GUI entfernen und alle vorhandenen Daten wurden auf die restlichen Platten aufgeteilt, doch wollte ich anschließend eine zweite Festplatte entfernen die auch ohne Probleme durch die anderen abgedeckt wurde und hier lief es in einen Fehler.  
Fehlermeldung war ungefähr: "Festplatte kann nicht entfernt werden, fügen Sie erst eine neue hinzu um die Daten zu übertragen"  
[![Storage-Spaces-001.jpg](https://wiki.eidolf.de/images/e/e9/Storage-Spaces-001.jpg)](https://wiki.eidolf.de/index.php/Datei:Storage-Spaces-001.jpg)  

  
Mit PowerShell konnte man die Platte aber folgendermaßen entfernen.

1. Überprüfen welche Festplatten vorhanden sind und von der zu entfernenden die ID aufschreiben bzw. merken 
`Get-PhysicalDisk`
2. Die Disk auf **Veraltet** setzen
`Get-PhysicalDisk | where {$_.DeviceId -eq "ID hier rein"} | Set-PhysicalDisk -Usage retired`
3. Den virtuellen Verbunden reparieren von dem die Platte entfernt wurde
`Repair-VirtualDisk -FriendlyName "Virtueller-Disk-Verbund"`
4. Danach kann die Disk auch über die GUI entfernt werden

# Rollen

## Active Directory

### FSMO Rollen lassen sich nicht übertragen

Nach etwas forschen in der Ereignisanzeige fallen folgende Fehler auf

- Ereignis 2213
- Ereignis 4012 (War zusätzlich bei mir vorhanden)

Als erstes musste ich Fehler 4012 loswerden (zu lange keine Synchronisierung des SYSVOL Shares über DFSR)  
`wmic.exe /namespace:\\root\microsoftdfs path DfsrMachineConfig set MaxOfflineTimeInDays=Tage eingeben über den geforderten in Fehler 2213`  
Als zweites den Befehl aus Ereignis 2213 ausführen  
`wmic /namespace:\\root\microsoftdfs path dfsrVolumeConfig where volumeGuid="GUID aus der Fehlermeldung" call ResumeReplication`  
Zum Schluss wenn die Replikation funktioniert (Share sollte an allen DC´s erscheinen) die **MaxOfflineTimeInDays** wieder auf Standard (60 Tage) zurücksetzen  
Diese Fehlerbereinigung nur durchführen falls alle anderen Domaincontroller überhaupt keinen Share haben!

### Migration von FSMO Rollen

Folgendes Beispiel ist eine Migration der Rollen von Server 2012 R2 zu 2016 oder neuer.

#### Voraussetzungen:

1. Neuer Domaincontroller vorhanden
2. Angemeldet am neuen DC als **Domain Admin** dem zusätzlich temporär die **Schema Admin** Rolle zugewiesen wird.
3. Am neuen DC eine Administrative Powershell offen.

#### Durchführung:

1. Prüfen des bisherigen Domain und Forest Function Level:
	1.1. Forest:`Get-ADForest | fl Name,ForestMode`
	1.2. Domain:`Get-ADDomain | fl Name,DomainMode`
2. Falls die Level unter den ältesten Serverversionen ist würde ich eine Anhebung auf die älteste Serverversion durchführen. 
	Somit bei Server 2012 R2 auf ebendieses. 
	2.1. [Anheben des Forest Function Levels](/de/Wiki-Seiten/Microsoft/Server/Rollen/AD/ad-anheben-des-forest-function-levels)
	2.2. [Anheben des Domain Function Levels](/de/Wiki-Seiten/Microsoft/Server/Rollen/AD/ad-anheben-des-domain-function-levels)
3. Mit `netdom query fsmo` überprüfen welcher Domaincontroller die jeweiligen Rollen besitzt.
4. Folgenden Befehl ausführen um alle Rollen zu "SRV-DCNeu01" zu verschieben
`Move-ADDirectoryServerOperationMasterRole -Identity SRV-DCNeu01 -OperationMasterRole SchemaMaster, DomainNamingMaster, PDCEmulator, RIDMaster, InfrastructureMaster`

#### Quelle:
https://blogs.technet.microsoft.com/canitpro/2017/05/24/step-by-step-migrating-active-directory-fsmo-roles-from-windows-server-2012-r2-to-2016/

### Wiederherstellen von AD Objekten aus einem Backup

Falls falsche Werte durch z.B. ein Script über viele AD Objekte verteilt wurde kann man über ein vorhandenes Backup den Altbestand bzw. sauberen Bestand parallel öffnen und mit Scripten den Livestand bearbeiten.  
Im Fall man hat ein Windows Backup mit Boardmitteln durchgeführt und somit eine VHDX Datei des Domaincontrollers vorliegen kann man folgendermaßen einzelne Objekte wiederherstellen.

1. Backup VHD einbinden bis man Zugriff auf die Windows Partition hat.
2. Administrativ eine CMD öffnen
3. Die AD Datenbank mit einen zusätzlichen Port einbinden `dsamain -dbpath "G:\Windows\NTDS\ntds.dit" -ldapport 10389` und danach das CMD Fenster geöffnet lassen. (Durch ein schließen des Fensters wird auch die Datenbankverbindung geschlossen)
4. Ab jetzt ist der Zugriff auf das AD mit **Servername:10389** gegeben.

#### Quelle:
https://stealthbits.com/blog/how-to-rollback-and-recover-active-directory-object-attributes/

# Windows Update

## Windows Update bleibt bei 0 Prozent hängen

KB3200970 muss von Hand installiert werden, danach sollten die Updates wieder funktionieren. Bei mir ist es bei einem Server aufgetreten, bei dem ich ein Upgrade durchgeführt habe. Herunterladen kann man das Update über den [Windows Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=KB3200970%7C)

### Quelle:
https://www.antary.de/2016/12/09/windows-server-2016-update-haengt-bei-0/