# Windows Server 2016

# <span class="mw-headline" id="bkmrk-features-1">Features</span>

## <span class="mw-headline" id="bkmrk-storage-spaces-1">Storage Spaces</span>

### <span class="mw-headline" id="bkmrk-schreibgeschwindigke-1">Schreibgeschwindigkeit schlecht</span>

Falls die Schreibgeschwindigkeit bei **Storage Spaces mit Parity** sehr schlecht sind, sollten alles Physikalischen Festplatten im Gerätemanager überprüft werden.  
Gerätemanager &gt; Laufwerke &gt; Eigenschaften der HDD &gt; Richtlinien  
Es sollte "Schreibcache auf dem Gerät aktivieren" angehakt sein. Weiterhin muss auf **!eigenes Risiko!** dem Storage Space manuell vorgegeben werden das eine USV vorhanden ist.

<div class="vector-body" id="bkmrk-administrative-power"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Administrative PowerShell öffnen
2. `Set-StoragePool -FriendlyName StoragePoolName-IsPowerProtected $true`

</div></div></div>Insgesamt verbessert sich dadurch die Leistung maximal auf das doppelte, leider ist der Wert immer noch nicht gut.

### <span class="mw-headline" id="bkmrk-festplatte-entfernen-1">Festplatte entfernen aus Verbund</span>

Ich konnte eine Festplatte ohne Probleme über GUI entfernen und alle vorhandenen Daten wurden auf die restlichen Platten aufgeteilt, doch wollte ich anschließend eine zweite Festplatte entfernen die auch ohne Probleme durch die anderen abgedeckt wurde und hier lief es in einen Fehler.  
Fehlermeldung war ungefähr: "Festplatte kann nicht entfernt werden, fügen Sie erst eine neue hinzu um die Daten zu übertragen"  
[![Storage-Spaces-001.jpg](https://wiki.eidolf.de/images/e/e9/Storage-Spaces-001.jpg)](https://wiki.eidolf.de/index.php/Datei:Storage-Spaces-001.jpg)  
  
Mit PowerShell konnte man die Platte aber folgendermaßen entfernen.

<div class="vector-body" id="bkmrk-%C3%9Cberpr%C3%BCfen-welche-fe"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Überprüfen welche Festplatten vorhanden sind und von der zu entfernenden die ID aufschreiben bzw. merken <dl><dd>`Get-PhysicalDisk`</dd></dl>
2. Die Disk auf **Veraltet** setzen <dl><dd>`Get-PhysicalDisk | where {$_.DeviceId -eq "ID hier rein"} | Set-PhysicalDisk -Usage retired`</dd></dl>
3. Den virtuellen Verbunden reparieren von dem die Platte entfernt wurde <dl><dd>`Repair-VirtualDisk -FriendlyName "Virtueller-Disk-Verbund"`</dd></dl>
4. Danach kann die Disk auch über die GUI entfernt werden

</div></div></div># <span class="mw-headline" id="bkmrk-rollen-1">Rollen</span>

## <span class="mw-headline" id="bkmrk-active-directory-1">Active Directory</span>

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-fsmo-rollen-lassen-s-1">FSMO Rollen lassen sich nicht übertragen</span>

Nach etwas forschen in der Ereignisanzeige fallen folgende Fehler auf

<div class="vector-body" id="bkmrk-ereignis-2213-ereign"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Ereignis 2213
- Ereignis 4012 (War zusätzlich bei mir vorhanden)

</div></div></div>Als erstes musste ich Fehler 4012 loswerden (zu lange keine Synchronisierung des SYSVOL Shares über DFSR)  
`wmic.exe /namespace:\\root\microsoftdfs path DfsrMachineConfig set MaxOfflineTimeInDays=Tage eingeben über den geforderten in Fehler 2213`  
Als zweites den Befehl aus Ereignis 2213 ausführen  
`wmic /namespace:\\root\microsoftdfs path dfsrVolumeConfig where volumeGuid="GUID aus der Fehlermeldung" call ResumeReplication`  
Zum Schluss wenn die Replikation funktioniert (Share sollte an allen DC´s erscheinen) die **MaxOfflineTimeInDays** wieder auf Standard (60 Tage) zurücksetzen  
Diese Fehlerbereinigung nur durchführen falls alle anderen Domaincontroller überhaupt keinen Share haben!

### <span class="mw-headline" id="bkmrk-migration-von-fsmo-r-1">Migration von FSMO Rollen</span>

Folgendes Beispiel ist eine Migration der Rollen von Server 2012 R2 zu 2016 oder neuer.

#### <span class="mw-headline" id="bkmrk-voraussetzungen%3A-1">Voraussetzungen:</span>

<div class="vector-body" id="bkmrk-neuer-domaincontroll"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Neuer Domaincontroller vorhanden
2. Angemeldet am neuen DC als **Domain Admin** dem zusätzlich temporär die **Schema Admin** Rolle zugewiesen wird.
3. Am neuen DC eine Administrative Powershell offen.

</div></div></div>#### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-durchf%C3%BChrung%3A-1">Durchführung:</span>

<div class="vector-body" id="bkmrk-pr%C3%BCfen-des-bisherige"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Prüfen des bisherigen Domain und Forest Function Level: 
    1. Forest:`Get-ADForest | fl Name,ForestMode`
    2. Domain:`Get-ADDomain | fl Name,DomainMode`
2. Falls die Level unter den ältesten Serverversionen ist würde ich eine Anhebung auf die älteste Serverversion durchführen.  
    Somit bei Server 2012 R2 auf ebendieses. 
    1. [Anheben des Forest Function Levels](https://wiki.eidolf.de/index.php/Anheben_des_Forest_Function_Levels "Anheben des Forest Function Levels")
    2. [Anheben des Domain Function Levels](https://wiki.eidolf.de/index.php/Anheben_des_Domain_Function_Levels "Anheben des Domain Function Levels")
3. Mit `netdom query fsmo` überprüfen welcher Domaincontroller die jeweiligen Rollen besitzt.
4. Folgenden Befehl ausführen um alle Rollen zu "SRV-DCNeu01" zu verschieben <dl><dd>`Move-ADDirectoryServerOperationMasterRole -Identity SRV-DCNeu01 -OperationMasterRole SchemaMaster, DomainNamingMaster, PDCEmulator, RIDMaster, InfrastructureMaster`</dd></dl>

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://blogs.technet.microsoft.com/canitpro/2017/05/24/step-by-step-migrating-active-directory-fsmo-roles-from-windows-server-2012-r2-to-2016/" rel="nofollow">https://blogs.technet.microsoft.com/canitpro/2017/05/24/step-by-step-migrating-active-directory-fsmo-roles-from-windows-server-2012-r2-to-2016/</a>
```

### <span class="mw-headline" id="bkmrk-wiederherstellen-von-1">Wiederherstellen von AD Objekten aus einem Backup</span>

Falls falsche Werte durch z.B. ein Script über viele AD Objekte verteilt wurde kann man über ein vorhandenes Backup den Altbestand bzw. sauberen Bestand parallel öffnen und mit Scripten den Livestand bearbeiten.  
Im Fall man hat ein Windows Backup mit Boardmitteln durchgeführt und somit eine VHDX Datei des Domaincontrollers vorliegen kann man folgendermaßen einzelne Objekte wiederherstellen.

<div class="vector-body" id="bkmrk-backup-vhd-einbinden"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Backup VHD einbinden bis man Zugriff auf die Windows Partition hat.
2. Administrativ eine CMD öffnen
3. Die AD Datenbank mit einen zusätzlichen Port einbinden `dsamain -dbpath "G:\Windows\NTDS\ntds.dit" -ldapport 10389` und danach das CMD Fenster geöffnet lassen. (Durch ein schließen des Fensters wird auch die Datenbankverbindung geschlossen)
4. Ab jetzt ist der Zugriff auf das AD mit **Servername:10389** gegeben.

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://stealthbits.com/blog/how-to-rollback-and-recover-active-directory-object-attributes/" rel="nofollow">https://stealthbits.com/blog/how-to-rollback-and-recover-active-directory-object-attributes/</a>
```

# <span class="mw-headline" id="bkmrk-windows-update-1">Windows Update</span>

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-windows-update-bleib-1">Windows Update bleibt bei 0 Prozent hängen</span>

KB3200970 muss von Hand installiert werden, danach sollten die Updates wieder funktionieren. Bei mir ist es bei einem Server aufgetreten, bei dem ich ein Upgrade durchgeführt habe. Herunterladen kann man das Update über den [Windows Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=KB3200970%7C)

### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://www.antary.de/2016/12/09/windows-server-2016-update-haengt-bei-0/" rel="nofollow">https://www.antary.de/2016/12/09/windows-server-2016-update-haengt-bei-0/</a>
```