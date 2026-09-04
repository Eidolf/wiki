---
title: Defekten Domänencontroller aus AD entfernen
description: Anleitung zum entfernen eines defekten Domänencontrollers aus dem Active Directory
published: true
date: 2026-09-04T13:35:01.074Z
tags: ad, dc, domain controller, korrupt, defekt
editor: markdown
dateCreated: 2026-09-03T15:23:16.739Z
---

# Nicht mehr verfügbaren Domänencontroller aus Active Directory entfernen

Diese Anleitung beschreibt das sichere Entfernen eines Domänencontrollers, der dauerhaft ausgefallen ist, nicht mehr gestartet werden kann oder physisch beziehungsweise virtuell nicht mehr existiert. Der Vorgang wird als **erzwungene Entfernung mit Metadatenbereinigung** bezeichnet.

> **Wichtig:** Führen Sie diese Schritte nur aus, wenn der ausgefallene Domänencontroller definitiv nicht mehr normal gestartet und herabgestuft werden kann. Nach der Bereinigung darf die alte Installation des Domänencontrollers nicht wieder mit dem Netzwerk verbunden werden.
{.is-warning}


# 1. Voraussetzungen und wichtige Sicherheitsprüfung

Für die Arbeiten benötigen Sie normalerweise:

- Ein Konto mit Mitgliedschaft in **Domänen-Admins**
- Für Änderungen an gesamtstrukturweiten FSMO-Rollen gegebenenfalls **Organisations-Admins**
- Zugriff auf mindestens einen funktionierenden, beschreibbaren Domänencontroller
- Die Active-Directory-Verwaltungstools beziehungsweise RSAT
- Eine administrative PowerShell oder Eingabeaufforderung

## Entscheidend: Existiert noch ein funktionierender Domänencontroller?

Führen Sie die Metadatenbereinigung nur aus, wenn mindestens ein anderer Domänencontroller der Domäne ordnungsgemäß funktioniert.

Falls der ausgefallene Server der **einzige Domänencontroller** der Domäne war, darf er nicht einfach aus Active Directory entfernt werden. In diesem Fall benötigen Sie eine Wiederherstellung des Domänencontrollers beziehungsweise der Gesamtstruktur aus einer geeigneten Systemstatussicherung.

## Vorhandene Domänencontroller anzeigen

Auf einem funktionierenden Domänencontroller oder einem Verwaltungsrechner mit installiertem Active-Directory-Modul:

```powershell
Get-ADDomainController -Filter * |
    Select-Object HostName, Site, IPv4Address, IsGlobalCatalog, OperationMasterRoles
```

Prüfen Sie zusätzlich, ob der noch vorhandene Domänencontroller die Domäne lesen und schreiben kann:

```powershell
Get-ADDomain
Get-ADForest
```

# 2. Zustand der Replikation prüfen

Öffnen Sie eine Eingabeaufforderung oder PowerShell als Administrator:

```cmd
repadmin /replsummary
```

Anschließend:

```cmd
repadmin /showrepl *
```

Der ausgefallene Domänencontroller wird wahrscheinlich mit Replikationsfehlern angezeigt. Entscheidend ist, dass die verbleibenden Domänencontroller untereinander erfolgreich replizieren.

Eine ausführlichere Übersicht kann mit folgendem Befehl erzeugt werden:

```cmd
repadmin /showrepl * /csv
```

Prüfen Sie außerdem die Erreichbarkeit der Domänendienste:

```cmd
dcdiag /e /c /v
```

Für eine gezielte Prüfung des DNS-Dienstes:

```cmd
dcdiag /test:dns /e /v
```

> Es ist nicht notwendig, dass alle Tests bezüglich des bereits ausgefallenen Domänencontrollers erfolgreich sind. Fehler zwischen den verbleibenden Domänencontrollern müssen jedoch vor oder unmittelbar nach der Bereinigung untersucht werden.

# 3. FSMO-Rollen prüfen

Ermitteln Sie, welche Domänencontroller aktuell die FSMO-Rollen besitzen:

```cmd
netdom query fsmo
```

Alternativ mit PowerShell:

```powershell
Get-ADDomain | Select-Object PDCEmulator, RIDMaster, InfrastructureMaster
Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster
```

Die fünf FSMO-Rollen sind:

- Schemamaster
- Domänennamenmaster
- PDC-Emulator
- RID-Master
- Infrastrukturmaster

## Falls der ausgefallene Domänencontroller FSMO-Rollen besitzt

Wenn der alte Domänencontroller definitiv nicht zurückkehren wird, müssen die betroffenen Rollen auf einen funktionierenden Domänencontroller **erzwungen übertragen**, also übernommen, werden.

Ersetzen Sie `DC02` durch den Namen des funktionierenden Ziel-Domänencontrollers.

Beispiel für die Übernahme aller Rollen:

```powershell
Move-ADDirectoryServerOperationMasterRole `
    -Identity "DC02" `
    -OperationMasterRole SchemaMaster,DomainNamingMaster,PDCEmulator,RIDMaster,InfrastructureMaster `
    -Force
```

Übernehmen Sie möglichst nur die Rollen, die tatsächlich auf dem ausgefallenen Server lagen. Beispiel für PDC-Emulator und RID-Master:

```powershell
Move-ADDirectoryServerOperationMasterRole `
    -Identity "DC02" `
    -OperationMasterRole PDCEmulator,RIDMaster `
    -Force
```

Danach erneut prüfen:

```cmd
netdom query fsmo
```

> Eine mit `-Force` übernommene FSMO-Rolle darf nicht wieder gleichzeitig vom alten Domänencontroller angeboten werden. Der alte Domänencontroller muss dauerhaft offline bleiben oder vor einer erneuten Verwendung vollständig neu installiert werden.

# 4. Prüfen, ob der ausgefallene Server zusätzliche Rollen hatte

Vor der Entfernung sollten Sie feststellen, ob der Server neben Active Directory weitere wichtige Aufgaben erfüllte:

- DNS-Server
- Globaler Katalog
- DHCP-Server
- Zertifizierungsstelle
- NPS beziehungsweise RADIUS
- Dateiserver oder DFS-Namespace
- DFS-Replikation außerhalb von SYSVOL
- Windows-Zeitquelle
- ADFS oder Web Application Proxy
- Entra-Connect-Synchronisierung
- Anwendungen mit fest eingetragenem LDAP-Server
- Backup-, Monitoring- oder Verwaltungsserver

## Globalen Katalog prüfen

```powershell
Get-ADDomainController -Filter * |
    Select-Object HostName, IsGlobalCatalog, Site
```

In jedem Standort sollte nach Möglichkeit ein erreichbarer globaler Katalog vorhanden sein. Dieser muss sich jedoch nicht zwingend in derselben Site befinden. Ein Domänencontroller gehört immer genau zu der Site, in der sich sein Serverobjekt befindet, und sollte nicht zur Abdeckung einer anderen Site verschoben oder mehrfach zugewiesen werden.

Falls erforderlich, kann ein verbleibender Domänencontroller als globaler Katalog konfiguriert werden:

1. Öffnen Sie `dssite.msc`.
2. Navigieren Sie zu **Standorte**.
3. Öffnen Sie den Standort und anschließend **Servers**.
4. Öffnen Sie den funktionierenden Domänencontroller.
5. Klicken Sie mit der rechten Maustaste auf **NTDS Settings**.
6. Wählen Sie **Eigenschaften**.
7. Aktivieren Sie **Globaler Katalog**.

Besitzt eine Site keinen eigenen Domänencontroller beziehungsweise globalen Katalog, verwenden die Clients automatisch einen erreichbaren Server aus einer anderen Site. Prüfen in diesem Fall:

- Das Client-IP-Netz ist unter **Subnets** der richtigen Site zugeordnet.
- Die Site ist über einen geeigneten **Site Link** angebunden.
- Die Site-Link-Kosten bevorzugen den gewünschten Standort.
- Der erreichbare Domänencontroller ist als globaler Katalog konfiguriert.

Die Auswahl kann auf einem Client der betroffenen Site geprüft werden:

```cmd
nltest /dsgetsite
nltest /dsgetdc:deinedomaene.local /force
nltest /dsgetdc:deinedomaene.local /GC /force
```

Folgender Befehl ist für die Forest Root Domain Prüfung, die eventuell unterschiedlich ist.
```cmd
nltest /dsgetdc:deinerootdomaene.local /GC /force
```

**Kurz gesagt:** Den Domänencontroller nicht verschieben oder doppelt zuweisen, sondern Subnetze, Site Links und deren Kosten korrekt konfigurieren.


# 5. Metadaten über Active Directory-Benutzer und -Computer entfernen

Dies ist bei aktuellen Windows-Server-Versionen der bevorzugte Weg.

1. Melden Sie sich an einem funktionierenden Domänencontroller oder Verwaltungsrechner an.
2. Öffnen Sie **Active Directory-Benutzer und -Computer**:

```cmd
dsa.msc
```

3. Aktivieren Sie unter **Ansicht** die Option **Erweiterte Features**.
4. Öffnen Sie die Organisationseinheit **Domain Controllers**.
5. Suchen Sie das Computerkonto des ausgefallenen Domänencontrollers.
6. Klicken Sie mit der rechten Maustaste darauf und wählen Sie **Löschen**.
7. Bestätigen Sie die Warnung.
8. Wenn angezeigt, aktivieren Sie die Option sinngemäß:

   **Dieser Domänencontroller ist dauerhaft offline und kann nicht mehr mit dem Active Directory-Installationsassistenten herabgestuft werden.**

9. Bestätigen Sie die Entfernung.

Bei unterstützten Verwaltungstools entfernt dieser Vorgang normalerweise gleichzeitig die zugehörigen Domänencontroller-Metadaten einschließlich des NTDS-Settings-Objekts.

# 6. Einträge in Active Directory-Standorte und -Dienste kontrollieren

Öffnen Sie:

```cmd
dssite.msc
```

Navigieren Sie zu:

```text
Standorte
  <Name des Standorts>
    Servers
      <Name des ausgefallenen Domänencontrollers>
```

Prüfen Sie, ob der alte Server noch vorhanden ist.

## Falls noch ein NTDS-Settings-Objekt vorhanden ist

1. Klicken Sie mit der rechten Maustaste auf **NTDS Settings**.
2. Wählen Sie **Löschen**.
3. Aktivieren Sie bei entsprechender Nachfrage die Bestätigung, dass der Domänencontroller dauerhaft offline ist.
4. Löschen Sie anschließend das nun leere Serverobjekt.

> Löschen Sie nicht unüberlegt manuell einzelne Replikationsverbindungen. Entfernen Sie zuerst das NTDS-Settings-Objekt und danach das Serverobjekt des ausgefallenen Domänencontrollers.

# 7. Alternative Metadatenbereinigung mit `ntdsutil`

Verwenden Sie diese Methode, wenn die grafische Bereinigung nicht möglich ist oder verwaiste Metadaten verbleiben.

Öffnen Sie eine Eingabeaufforderung als Administrator:

```cmd
ntdsutil
```

Wechseln Sie zur Metadatenbereinigung:

```cmd
metadata cleanup
```

Stellen Sie eine Verbindung zu einem funktionierenden Domänencontroller her:

```cmd
connections
connect to server DC02
quit
```

Ersetzen Sie `DC02` durch den Namen eines funktionierenden Domänencontrollers.

Wählen Sie anschließend das zu entfernende Serverobjekt aus:

```cmd
select operation target
list domains
select domain <Nummer>
list sites
select site <Nummer>
list servers in site
select server <Nummer>
quit
```

Kontrollieren Sie sehr sorgfältig, dass der ausgewählte Server tatsächlich der ausgefallene Domänencontroller ist. Entfernen Sie ihn anschließend:

```cmd
remove selected server
```

Bestätigen Sie die Rückfragen und beenden Sie das Programm:

```cmd
quit
quit
```

> Verwenden Sie `remove selected server` niemals, bevor Sie anhand der angezeigten Auswahl geprüft haben, dass wirklich der richtige Domänencontroller ausgewählt ist.

# 8. DNS-Einträge bereinigen

Ein ausgefallener Domänencontroller hinterlässt häufig veraltete DNS-Einträge. War der Domänencontroller gleichzeitig DNS-Server, können zusätzlich in Forward- und Reverse-Lookupzonen noch Nameserver-, SOA-, Delegierungs- oder Weiterleitungsverweise vorhanden sein.

Diese Einträge werden nach einer erzwungenen Entfernung des Domänencontrollers nicht immer vollständig automatisch gelöscht. Kontrollieren Sie deshalb:

- NS- und SOA-Einträge
- A-, AAAA- und PTR-Einträge
- SRV-Einträge
- den DSA-GUID-CNAME in `_msdcs`
- Delegierungen und Glue-Einträge
- Weiterleitungen, Stubzonen und Zonentransfers
- die DNS-Konfiguration von Clients und Servern

> **Wichtig:** Entfernen Sie ausschließlich Einträge, die eindeutig auf den ausgefallenen Domänencontroller beziehungsweise DNS-Server verweisen. Stellen Sie vor dem Entfernen von NS-Einträgen sicher, dass für jede betroffene DNS-Zone mindestens ein funktionierender autoritativer DNS-Server verbleibt.

## 8.1 Funktionierenden DNS-Server und alte Serverdaten festlegen

Führen Sie die folgenden Arbeiten auf einem verbleibenden und funktionierenden DNS-Server beziehungsweise Domänencontroller aus.

Öffnen Sie eine administrative PowerShell-Sitzung und laden Sie das DNS-Server-Modul:

```powershell
Import-Module DnsServer
```

Legen Sie anschließend den abzufragenden DNS-Server sowie die Daten des ausgefallenen Domänencontrollers fest:

```powershell
$DnsServer = $env:COMPUTERNAME
$OldServerName = "DC-ALT"
$OldServerFqdn = "DC-ALT.contoso.local"
$OldServerIPv4 = "192.168.10.10"
```

Ersetzen Sie die Beispielwerte durch die tatsächlichen Daten:

- `$DnsServer` bezeichnet den funktionierenden DNS-Server, auf dem die Bereinigung ausgeführt wird.
- `$OldServerName` bezeichnet den kurzen Namen des ausgefallenen Domänencontrollers.
- `$OldServerFqdn` bezeichnet dessen vollständigen DNS-Namen.
- `$OldServerIPv4` bezeichnet dessen bisherige IPv4-Adresse.

Soll die Bereinigung remote auf einem anderen DNS-Server erfolgen, tragen Sie dessen Namen explizit ein:

```powershell
$DnsServer = "DC01.contoso.local"
```

Bereiten Sie den vollständigen Namen für zuverlässige Vergleiche vor:

```powershell
$OldServerFqdnNormalized = $OldServerFqdn.TrimEnd(".").ToLowerInvariant()
```

## 8.2 Vorhandene DNS-Zonen erfassen

Zeigen Sie zunächst die auf dem ausgewählten DNS-Server vorhandenen Zonen an:

```powershell
Get-DnsServerZone -ComputerName $DnsServer |
    Select-Object ZoneName, ZoneType, IsDsIntegrated, IsReverseLookupZone |
    Sort-Object IsReverseLookupZone, ZoneName |
    Format-Table -AutoSize
```

Prüfen Sie insbesondere:

- AD-integrierte Forward-Lookupzonen
- AD-integrierte Reverse-Lookupzonen
- primäre, nicht AD-integrierte Zonen
- delegierte Zonen
- die Zone `_msdcs.<Gesamtstruktur-Stammdomäne>`

Erfassen Sie anschließend alle für die Suche geeigneten Zonen:

```powershell
$Zones = Get-DnsServerZone -ComputerName $DnsServer |
    Where-Object {
        -not $_.IsAutoCreated -and
        $_.ZoneName -notin @(
            "TrustAnchors",
            "..TrustAnchors"
        )
    }
```

Bei AD-integrierten Zonen genügt die Änderung normalerweise auf einem beschreibbaren DNS-Domänencontroller, auf dem die betreffende Zone verfügbar ist. Die Änderung wird anschließend über Active Directory repliziert.

Nicht AD-integrierte primäre Zonen müssen auf dem jeweils zuständigen primären DNS-Server bearbeitet werden. Sekundäre Zonen beziehen ihre Daten normalerweise über Zonentransfers und sollten nicht unabhängig manuell bereinigt werden.

## 8.3 Veraltete Nameserver-Einträge zonenübergreifend suchen

War der ausgefallene Domänencontroller gleichzeitig DNS-Server, kann er in mehreren Zonen weiterhin als autoritativer Nameserver eingetragen sein. Im DNS-Manager werden diese Einträge in den Eigenschaften einer Zone auf der Registerkarte **Nameserver** angezeigt.

Durchsuchen Sie alle geeigneten Zonen nach NS-Einträgen, die auf den ausgefallenen Server verweisen:

```powershell
$OldNsRecords = foreach ($Zone in $Zones) {
    try {
        $Records = Get-DnsServerResourceRecord `
            -ComputerName $DnsServer `
            -ZoneName $Zone.ZoneName `
            -RRType NS `
            -ErrorAction Stop

        foreach ($Record in $Records) {
            $NameServer = $Record.RecordData.NameServer.ToString().
                TrimEnd(".").
                ToLowerInvariant()

            if ($NameServer -eq $OldServerFqdnNormalized) {
                [PSCustomObject]@{
                    ZoneName           = $Zone.ZoneName
                    ZoneType           = $Zone.ZoneType
                    IsDsIntegrated      = $Zone.IsDsIntegrated
                    IsReverseLookupZone = $Zone.IsReverseLookupZone
                    HostName           = $Record.HostName
                    NameServer         = $Record.RecordData.NameServer
                    RecordObject       = $Record
                }
            }
        }
    }
    catch {
        Write-Warning (
            "Zone '{0}' konnte nicht gelesen werden: {1}" -f
            $Zone.ZoneName,
            $_.Exception.Message
        )
    }
}
```

Zeigen Sie die gefundenen Einträge an:

```powershell
$OldNsRecords |
    Select-Object ZoneName,
                  ZoneType,
                  IsDsIntegrated,
                  IsReverseLookupZone,
                  HostName,
                  NameServer |
    Sort-Object ZoneName, HostName |
    Format-Table -AutoSize
```

Die Suche berücksichtigt:

- Forward-Lookupzonen
- IPv4-Reverse-Lookupzonen
- IPv6-Reverse-Lookupzonen
- NS-Einträge am Stamm einer Zone
- NS-Einträge innerhalb von DNS-Delegierungen

## 8.4 Gefundene Nameserver-Einträge protokollieren

Erstellen Sie vor der Entfernung ein Protokoll der gefundenen Einträge:

```powershell
$LogDirectory = "C:\Temp"
$LogFile = Join-Path `
    -Path $LogDirectory `
    -ChildPath "DNS-NS-Bereinigung-$OldServerName.csv"

New-Item `
    -Path $LogDirectory `
    -ItemType Directory `
    -Force |
    Out-Null

$OldNsRecords |
    Select-Object ZoneName,
                  ZoneType,
                  IsDsIntegrated,
                  IsReverseLookupZone,
                  HostName,
                  NameServer |
    Export-Csv `
        -Path $LogFile `
        -NoTypeInformation `
        -Encoding UTF8

Write-Host "Protokoll erstellt: $LogFile"
```

Prüfen Sie die CSV-Datei und die Bildschirmausgabe sorgfältig. Entfernt werden dürfen nur Einträge, deren `NameServer` eindeutig dem ausgefallenen Domänencontroller entspricht.

Ein leerer Export beziehungsweise eine leere Anzeige bedeutet, dass unter dem angegebenen vollständigen DNS-Namen keine passenden NS-Einträge gefunden wurden.

## 8.5 Delegierungen besonders prüfen

NS-Einträge können sich entweder am Stamm einer Zone oder an einem untergeordneten Knoten befinden. Ein Eintrag an einem untergeordneten Knoten kann Bestandteil einer DNS-Delegierung sein.

Beispiel:

```text
Zone: contoso.local
Knoten: niederlassung
Nameserver: DC-ALT.contoso.local
```

Dieser Eintrag kann die folgende delegierte Zone betreffen:

```text
niederlassung.contoso.local
```

Prüfen Sie Einträge, deren `HostName` nicht dem Zonenstamm entspricht, besonders sorgfältig.

Bei einer Delegierung können zwei Bestandteile vorhanden sein:

1. Der NS-Eintrag, der auf den ausgefallenen DNS-Server verweist.
2. Ein zugehöriger Glue-A- oder Glue-AAAA-Eintrag.

Ein Glue-Eintrag darf erst entfernt werden, wenn sichergestellt ist, dass er nicht mehr für eine weiterhin benötigte Delegierung verwendet wird.

## 8.6 Entfernung der Nameserver-Einträge simulieren

Führen Sie die Entfernung zunächst ausschließlich mit `-WhatIf` aus:

```powershell
foreach ($Entry in $OldNsRecords) {
    $CurrentNameServer = $Entry.RecordObject.RecordData.NameServer.
        ToString().
        TrimEnd(".").
        ToLowerInvariant()

    if ($CurrentNameServer -ne $OldServerFqdnNormalized) {
        Write-Warning (
            "Eintrag übersprungen: Zone='{0}', Knoten='{1}', Nameserver='{2}'" -f
            $Entry.ZoneName,
            $Entry.HostName,
            $Entry.NameServer
        )

        continue
    }

    Write-Host ""
    Write-Host "Geplante Entfernung:" -ForegroundColor Yellow
    Write-Host "  DNS-Server : $DnsServer"
    Write-Host "  Zone       : $($Entry.ZoneName)"
    Write-Host "  Knoten     : $($Entry.HostName)"
    Write-Host "  Record-Typ : NS"
    Write-Host "  Nameserver : $($Entry.NameServer)"

    Remove-DnsServerResourceRecord `
        -ComputerName $DnsServer `
        -ZoneName $Entry.ZoneName `
        -InputObject $Entry.RecordObject `
        -WhatIf
}
```

Kontrollieren Sie die Ausgabe vollständig. Durch die Verwendung von `-InputObject` wird genau das zuvor gefundene und geprüfte Datensatzobjekt angesprochen.

## 8.7 Veraltete Nameserver-Einträge entfernen

Wenn die Ausgabe von `-WhatIf` ausschließlich die erwarteten Einträge enthält, führen Sie die tatsächliche Entfernung aus:

```powershell
foreach ($Entry in $OldNsRecords) {
    try {
        Remove-DnsServerResourceRecord `
            -ComputerName $DnsServer `
            -ZoneName $Entry.ZoneName `
            -InputObject $Entry.RecordObject `
            -Force `
            -ErrorAction Stop

        Write-Host (
            "Entfernt: Zone '{0}', Knoten '{1}', Nameserver '{2}'" -f
            $Entry.ZoneName,
            $Entry.HostName,
            $Entry.NameServer
        )
    }
    catch {
        Write-Warning (
            "Eintrag in Zone '{0}' konnte nicht entfernt werden: {1}" -f
            $Entry.ZoneName,
            $_.Exception.Message
        )
    }
}
```

> **Hinweis:** Schlägt die Entfernung in einzelnen Zonen fehl, prüfen Sie, ob es sich um eine sekundäre, schreibgeschützte oder nicht lokal verwaltete Zone handelt. Führen Sie die Änderung in diesem Fall auf dem zuständigen primären beziehungsweise beschreibbaren DNS-Server aus.

## 8.8 Nameserver-Bereinigung kontrollieren

Führen Sie nach der Entfernung die Suche erneut aus:

```powershell
$RemainingOldNsRecords = foreach ($Zone in $Zones) {
    try {
        Get-DnsServerResourceRecord `
            -ComputerName $DnsServer `
            -ZoneName $Zone.ZoneName `
            -RRType NS `
            -ErrorAction Stop |
            Where-Object {
                $_.RecordData.NameServer.ToString().
                    TrimEnd(".").
                    ToLowerInvariant() -eq $OldServerFqdnNormalized
            } |
            Select-Object @{
                Name = "ZoneName"
                Expression = { $Zone.ZoneName }
            }, HostName, @{
                Name = "NameServer"
                Expression = { $_.RecordData.NameServer }
            }
    }
    catch {
        Write-Warning (
            "Zone '{0}' konnte nicht kontrolliert werden." -f
            $Zone.ZoneName
        )
    }
}

$RemainingOldNsRecords |
    Format-Table -AutoSize
```

Wenn keine Einträge mehr ausgegeben werden, wurden auf dem untersuchten DNS-Server keine NS-Einträge mehr gefunden, die auf den alten Server verweisen.

## 8.9 SOA-Einträge kontrollieren

Zusätzlich zu den NS-Einträgen kann der ausgefallene DNS-Server noch im SOA-Eintrag einer Zone als primärer Server eingetragen sein.

Kontrollieren Sie die SOA-Einträge aller Zonen:

```powershell
$SoaRecords = foreach ($Zone in $Zones) {
    try {
        Get-DnsServerResourceRecord `
            -ComputerName $DnsServer `
            -ZoneName $Zone.ZoneName `
            -RRType SOA `
            -ErrorAction Stop |
            Select-Object @{
                Name = "ZoneName"
                Expression = { $Zone.ZoneName }
            }, @{
                Name = "ZoneType"
                Expression = { $Zone.ZoneType }
            }, @{
                Name = "PrimaryServer"
                Expression = { $_.RecordData.PrimaryServer }
            }, @{
                Name = "ResponsiblePerson"
                Expression = { $_.RecordData.ResponsiblePerson }
            }
    }
    catch {
        Write-Warning (
            "SOA-Eintrag der Zone '{0}' konnte nicht gelesen werden." -f
            $Zone.ZoneName
        )
    }
}

$SoaRecords |
    Sort-Object ZoneName |
    Format-Table -AutoSize
```

Filtern Sie die Ausgabe bei Bedarf nach dem alten Server:

```powershell
$SoaRecords |
    Where-Object {
        $_.PrimaryServer.ToString().
            TrimEnd(".").
            ToLowerInvariant() -eq $OldServerFqdnNormalized
    } |
    Format-Table -AutoSize
```

> **Wichtig:** Löschen Sie keinen SOA-Eintrag. Jede DNS-Zone benötigt einen SOA-Eintrag. Ist dort noch der ausgefallene Server eingetragen, muss der Eintrag auf einen funktionierenden autoritativen DNS-Server geändert werden.

Bei AD-integrierten Zonen kann die SOA-Anzeige durch die Multi-Master-Replikation und die aktuell abgefragte DNS-Serverinstanz beeinflusst werden. Kontrollieren Sie einen verbliebenen Eintrag deshalb nach Möglichkeit auf mehreren DNS-Servern, bevor Sie ihn manuell ändern.

## 8.10 DNS-Manager öffnen

Öffnen Sie zur manuellen Kontrolle den DNS-Manager:

```cmd
dnsmgmt.msc
```

Prüfen Sie sowohl die Forward-Lookupzonen als auch die Reverse-Lookupzonen.

## 8.11 Host- und Aliaseinträge entfernen

Entfernen Sie veraltete Einträge des ausgefallenen Servers:

- A-Einträge
- AAAA-Einträge
- zusätzliche CNAME- beziehungsweise Aliaseinträge, sofern diese eindeutig nur den alten Server betreffen

Suchen Sie sowohl nach dem kurzen Servernamen als auch nach dem vollständigen DNS-Namen:

```text
DC-ALT
DC-ALT.contoso.local
```

Wenn dieselbe IP-Adresse inzwischen einem anderen System zugewiesen wurde, darf nicht allein anhand der IP-Adresse gelöscht werden. Prüfen Sie zusätzlich den Namen und den Verwendungszweck des Eintrags.

## 8.12 PTR-Einträge in Reverse-Lookupzonen entfernen

Prüfen Sie alle relevanten Reverse-Lookupzonen auf PTR-Einträge des ausgefallenen Domänencontrollers. Entfernen Sie nur Einträge, die eindeutig auf dessen vollständigen DNS-Namen verweisen:

```text
DC-ALT.contoso.local
```

Berücksichtigen Sie:

- IPv4-Reverse-Lookupzonen unter `in-addr.arpa`
- IPv6-Reverse-Lookupzonen unter `ip6.arpa`
- mehrere Reverse-Lookupzonen bei mehreren Standorten oder Netzsegmenten

Die NS-Bereinigung entfernt ausschließlich Nameserver-Einträge. PTR-Einträge müssen separat kontrolliert und entfernt werden.

## 8.13 SRV-Einträge kontrollieren

Domänencontroller registrieren zahlreiche SRV-Einträge für LDAP, Kerberos, Kennwortänderungen, Global Catalog und standortbezogene Dienste.

Kontrollieren Sie insbesondere die Ordner:

```text
_ldap
_kerberos
_kpasswd
_gc
_tcp
_udp
_sites
```

Diese befinden sich üblicherweise innerhalb der DNS-Zonen:

```text
<Ihre-Domäne>
_msdcs.<Ihre-Gesamtstruktur>
```

Je nach Aufbau der DNS-Zonen können sich die Einträge unter anderem in folgenden Pfaden befinden:

```text
_tcp.<Ihre-Domäne>
_udp.<Ihre-Domäne>
_sites.<Ihre-Domäne>
_tcp.dc._msdcs.<Ihre-Domäne>
_sites.dc._msdcs.<Ihre-Domäne>
_tcp.gc._msdcs.<Ihre-Gesamtstruktur>
_sites.gc._msdcs.<Ihre-Gesamtstruktur>
```

Entfernen Sie nur SRV-Einträge, deren Ziel eindeutig auf den ausgefallenen Domänencontroller verweist:

```text
DC-ALT.contoso.local
```

Löschen Sie nicht pauschal vollständige Ordner wie `_ldap`, `_kerberos`, `_tcp`, `_udp`, `_gc` oder `_sites`. Diese enthalten normalerweise auch weiterhin benötigte Einträge der funktionierenden Domänencontroller.

## 8.14 DSA-GUID-CNAME in `_msdcs` entfernen

In der folgenden Zone kann ein CNAME-Eintrag mit der DSA-GUID des alten Domänencontrollers vorhanden sein:

```text
_msdcs.<Gesamtstruktur-Stammdomäne>
```

Der Eintrag hat ungefähr folgende Form:

```text
<DSA-GUID>._msdcs.contoso.local
```

Er verweist auf:

```text
DC-ALT.contoso.local
```

Wenn der CNAME-Eintrag eindeutig zum dauerhaft entfernten Domänencontroller gehört, sollte er entfernt werden.

Löschen Sie die GUID nicht allein anhand ihres Aussehens. Kontrollieren Sie immer das Ziel des CNAME-Eintrags und vergleichen Sie es mit dem vollständigen DNS-Namen des ausgefallenen Domänencontrollers.

## 8.15 Glue-Einträge und Delegierungen prüfen

Prüfen Sie in den übergeordneten DNS-Zonen vorhandene Delegierungen. Eine Delegierung kann weiterhin folgende Einträge enthalten:

- einen NS-Eintrag für den ausgefallenen DNS-Server
- einen zugehörigen A-Glue-Eintrag
- einen zugehörigen AAAA-Glue-Eintrag

Entfernen Sie veraltete Glue-Einträge erst, nachdem der dazugehörige NS-Eintrag entfernt oder auf einen funktionierenden DNS-Server geändert wurde.

Besondere Vorsicht ist erforderlich, wenn derselbe Hosteintrag noch von einer anderen funktionierenden Delegierung verwendet wird.

## 8.16 Eigenschaften der DNS-Zonen kontrollieren

Öffnen Sie die Eigenschaften jeder besonders relevanten DNS-Zone und prüfen Sie die Registerkarte **Nameserver**.

Diese manuelle Kontrolle dient nach der automatisierten Bereinigung hauptsächlich der Verifikation. Prüfen Sie insbesondere:

- die Domänenzone
- die Zone `_msdcs.<Gesamtstruktur-Stammdomäne>`
- wichtige Anwendungszonen
- Reverse-Lookupzonen
- delegierte Zonen

Der ausgefallene DNS-Server darf dort nicht mehr als autoritativer Nameserver angezeigt werden.

## 8.17 Weitere DNS-Verweise kontrollieren

War der ausgefallene Domänencontroller auch DNS-Server, prüfen Sie zusätzlich:

- DNS-Weiterleitungen
- bedingte Weiterleitungen
- Stubzonen
- sekundäre Zonen
- Zonentransferziele
- Benachrichtigungslisten für Zonentransfers
- DNS-Richtlinien, sofern verwendet
- Name-Resolution-Policies, sofern vorhanden

Entfernen oder ersetzen Sie Verweise auf den ausgefallenen DNS-Server.

Achten Sie bei bedingten Weiterleitungen darauf, ob diese in Active Directory gespeichert und auf weitere DNS-Server repliziert werden. In diesem Fall sollte die Änderung auf einem beschreibbaren DNS-Domänencontroller vorgenommen und anschließend die Replikation kontrolliert werden.

## 8.18 DNS-Konfiguration von Clients und Servern prüfen

Prüfen Sie, ob der ausgefallene DNS-Server noch per DHCP verteilt oder statisch verwendet wird.

Kontrollieren Sie insbesondere:

- DHCP-Option 006
- DHCP-Richtlinien
- DHCP-Bereichsoptionen
- DHCP-Serveroptionen
- statische Netzwerkkonfiguration von Servern
- Netzwerkkonfiguration der verbleibenden Domänencontroller
- Hypervisoren
- Netzwerkgeräte
- VPN-Konfigurationen
- Anwendungen und Appliances
- Drucker und Scanner
- Managementschnittstellen
- Backup- und Monitoring-Systeme
- Firewall- und Proxy-Konfigurationen

Aktuelle IPv4-DNS-Konfiguration anzeigen:

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4
```

Aktuelle IPv6-DNS-Konfiguration anzeigen:

```powershell
Get-DnsClientServerAddress -AddressFamily IPv6
```

Kompakte Anzeige aller Schnittstellen mit konfigurierten DNS-Servern:

```powershell
Get-DnsClientServerAddress |
    Where-Object {
        $_.ServerAddresses.Count -gt 0
    } |
    Select-Object InterfaceAlias, AddressFamily, ServerAddresses |
    Format-Table -AutoSize
```

Kontrollieren Sie besonders die DNS-Clientkonfiguration der verbleibenden Domänencontroller. Dort darf die IP-Adresse des ausgefallenen DNS-Servers nicht mehr als bevorzugter oder alternativer DNS-Server eingetragen sein.

## 8.19 DHCP-Konfiguration prüfen

Wird die DNS-Serveradresse über DHCP verteilt, prüfen Sie mindestens die DHCP-Option 006 auf folgenden Ebenen:

1. DHCP-Serveroptionen
2. Bereichsoptionen
3. DHCP-Richtlinien
4. Reservierungen mit abweichenden Optionen

Nach einer Änderung müssen Clients ihren DHCP-Lease gegebenenfalls erneuern:

```cmd
ipconfig /release
ipconfig /renew
```

> **Vorsicht:** `ipconfig /release` kann eine bestehende Remoteverbindung unterbrechen. Auf Servern mit statischer Netzwerkkonfiguration dürfen diese Befehle nicht verwendet werden.

## 8.20 Lokale DNS-Caches leeren

Nach den Änderungen kann auf Windows-Systemen der lokale DNS-Cache geleert werden:

```cmd
ipconfig /flushdns
```

Testen Sie anschließend, ob noch eine veraltete Namensauflösung vorhanden ist:

```cmd
nslookup DC-ALT.contoso.local
```

Für einen vollständig entfernten Server sollte keine veraltete Namensauflösung mehr zurückgegeben werden, sofern der Name nicht bewusst anderweitig weiterverwendet wird.

## 8.21 DNS-Einträge der verbleibenden Domänencontroller neu registrieren

Ein funktionierender Domänencontroller kann seine Hosteinträge erneut registrieren:

```cmd
ipconfig /registerdns
```

Zusätzlich kann der Netlogon-Dienst neu gestartet werden, um die domänenspezifischen DNS-Einträge erneut zu registrieren:

```powershell
Restart-Service Netlogon
```

Der Neustart des Netlogon-Dienstes sollte mit Bedacht und möglichst außerhalb kritischer Anmelde- oder Wartungsprozesse erfolgen.

## 8.22 DNS-Registrierung kontrollieren

Kontrollieren Sie nach der erneuten Registrierung die DNS-Einträge der verbleibenden Domänencontroller.

LDAP-Diensteinträge der Domäne abfragen:

```cmd
nslookup -type=SRV _ldap._tcp.dc._msdcs.contoso.local
```

Globale Kataloge über die Gesamtstruktur-Stammdomäne abfragen:

```cmd
nslookup -type=SRV _ldap._tcp.gc._msdcs.contoso.local
```

Kerberos-Diensteinträge kontrollieren:

```cmd
nslookup -type=SRV _kerberos._tcp.contoso.local
```

Ersetzen Sie `contoso.local` durch den tatsächlichen DNS-Namen der Domäne beziehungsweise der Gesamtstruktur-Stammdomäne.

Die zurückgegebenen Einträge dürfen nicht mehr auf den ausgefallenen Domänencontroller verweisen.

## 8.23 Active-Directory-Replikation ausführen und prüfen

Sind die DNS-Zonen Active-Directory-integriert, werden die Änderungen über Active Directory repliziert. Stoßen Sie die Replikation bei Bedarf auf einem verbleibenden Domänencontroller an:

```cmd
repadmin /syncall /AdeP
```

Prüfen Sie anschließend die Replikationsübersicht:

```cmd
repadmin /replsummary
```

Zeigen Sie bei Fehlern die detaillierten Replikationsverbindungen an:

```cmd
repadmin /showrepl *
```

Führen Sie die NS-, SRV- und Hosteintragskontrolle anschließend stichprobenartig auf einem weiteren DNS-Domänencontroller aus.

Vermeiden Sie es, dieselben AD-integrierten DNS-Einträge unmittelbar und parallel auf allen DNS-Servern zu löschen. Nehmen Sie die Bereinigung zunächst auf einem beschreibbaren DNS-Domänencontroller vor und prüfen Sie anschließend die ordnungsgemäße Replikation.

## 8.24 DNS-Diagnose ausführen

Führen Sie auf einem verbleibenden Domänencontroller eine DNS-Diagnose aus:

```cmd
dcdiag /test:dns /v
```

Für einen Gesamtüberblick über alle Domänencontroller der Gesamtstruktur:

```cmd
dcdiag /test:dns /e /v
```

Die ausführliche Ausgabe kann in eine Datei geschrieben werden:

```cmd
dcdiag /test:dns /e /v > C:\Temp\dcdiag-dns.txt
```

Prüfen Sie die Ausgabe insbesondere auf:

- fehlgeschlagene Namensauflösung
- fehlende SRV-Einträge
- veraltete Verweise auf den ausgefallenen Server
- Registrierungsfehler
- Delegierungsfehler
- Weiterleitungsprobleme
- nicht erreichbare DNS-Server

## 8.25 Abschließende Kontrolle

Suchen Sie abschließend auf den verbleibenden DNS-Servern nach:

- dem kurzen Namen des ausgefallenen Domänencontrollers
- dessen vollständigem DNS-Namen
- dessen bisheriger IPv4-Adresse
- dessen bisheriger IPv6-Adresse
- dessen DSA-GUID
- dessen alten NS-Einträgen
- dessen SRV-Zielen
- dessen PTR-Einträgen
- dessen Vorkommen in Delegierungen
- dessen Vorkommen in Weiterleitungen und Zonentransferkonfigurationen

Kontrollieren Sie mindestens folgende Bereiche:

```text
Forward-Lookupzonen
Reverse-Lookupzonen
_msdcs.<Gesamtstruktur-Stammdomäne>
Delegierungen
Bedingte Weiterleitungen
Sekundäre Zonen
Stubzonen
Zonentransferkonfigurationen
DHCP-Optionen
Statische DNS-Clientkonfigurationen
```

Die DNS-Bereinigung ist abgeschlossen, wenn:

- der ausgefallene Server nicht mehr als Nameserver einer Zone eingetragen ist,
- keine veralteten A-, AAAA- oder PTR-Einträge vorhanden sind,
- keine SRV-Einträge mehr auf ihn verweisen,
- sein DSA-GUID-CNAME aus `_msdcs` entfernt wurde,
- keine Weiterleitungen oder Delegierungen mehr auf ihn verweisen,
- Clients und Server ihn nicht mehr als DNS-Server verwenden,
- die verbleibenden Domänencontroller ihre DNS-Einträge korrekt registriert haben,
- die Active-Directory-Replikation ohne relevante Fehler funktioniert,
- und `dcdiag /test:dns` keine durch den entfernten Server verursachten Fehler mehr meldet.

# 9. Computerobjekt und weitere verwaiste Objekte kontrollieren

Prüfen Sie in **Active Directory-Benutzer und -Computer**, ob das Computerkonto des alten Domänencontrollers vollständig entfernt wurde.

Mit PowerShell:

```powershell
Get-ADComputer -Identity "DC01"
```

Wenn der Befehl meldet, dass das Objekt nicht gefunden wurde, ist das Computerkonto bereits entfernt.

Prüfen Sie zusätzlich, ob noch ein Domänencontrollerobjekt zurückgegeben wird:

```powershell
Get-ADDomainController -Identity "DC01"
```

Ersetzen Sie `DC01` durch den Namen des ausgefallenen Servers.

# 10. Replikation nach der Bereinigung anstoßen

Stoßen Sie die Replikation auf den verbleibenden Domänencontrollern an:

```cmd
repadmin /syncall /AdeP
```

Bedeutung der verwendeten Optionen:

- `/A`: Alle Namenskontexte
- `/d`: Servernamen in den Meldungen anzeigen
- `/e`: Standortübergreifende Replikation einbeziehen
- `/P`: Änderungen nach außen übertragen

Prüfen Sie danach erneut:

```cmd
repadmin /replsummary
```

Und:

```cmd
repadmin /showrepl *
```

Wenn nur noch ein Domänencontroller vorhanden ist, gibt es keinen Replikationspartner. In diesem Fall darf `repadmin` keine erfolgreiche Replikation zu einem zweiten Server erwarten lassen. Es sollten aber keine aktiven Verweise auf den entfernten Server bestehen bleiben.

# 11. Active Directory diagnostizieren

Führen Sie nach der Bereinigung eine Gesamtprüfung aus:

```cmd
dcdiag /e /c /v
```

DNS-Prüfung:

```cmd
dcdiag /test:dns /e /v
```

Prüfung der SYSVOL- und Netlogon-Freigaben:

```cmd
net share
```

Auf jedem verbleibenden Domänencontroller sollten mindestens die folgenden Freigaben vorhanden sein:

```text
NETLOGON
SYSVOL
```

Prüfen Sie außerdem den SYSVOL-Replikationsstatus:

```cmd
dcdiag /test:sysvolcheck /test:advertising
```

# 12. Ereignisprotokolle kontrollieren

Öffnen Sie die Ereignisanzeige:

```cmd
eventvwr.msc
```

Prüfen Sie insbesondere:

- Verzeichnisdienst
- DNS-Server
- DFS-Replikation
- System
- Active Directory Web Services

Achten Sie auf wiederkehrende Meldungen, die weiterhin den entfernten Domänencontroller, dessen GUID, dessen DNS-Namen oder seine IP-Adresse nennen.

Einzelne ältere Ereignisse sind nach der Bereinigung nicht automatisch problematisch. Entscheidend ist, ob neue Fehler weiterhin auftreten.

# 13. Zeitdienst kontrollieren

Wenn der ausgefallene Domänencontroller der PDC-Emulator oder die externe Zeitquelle war, muss die Zeitkonfiguration geprüft werden.

Aktuellen Status anzeigen:

```cmd
w32tm /query /status
```

Konfiguration anzeigen:

```cmd
w32tm /query /configuration
```

Quelle anzeigen:

```cmd
w32tm /query /source
```

Der PDC-Emulator der Gesamtstruktur-Stammdomäne sollte gegen eine zuverlässige externe Zeitquelle synchronisieren. Andere Domänenmitglieder folgen normalerweise der Active-Directory-Zeithierarchie.

# 14. DHCP kontrollieren

Falls der ausgefallene Server DHCP ausführte:

1. Prüfen Sie, ob ein anderer DHCP-Server vorhanden ist.
2. Prüfen Sie die DHCP-Autorisierung in Active Directory.
3. Entfernen Sie gegebenenfalls den alten DHCP-Server aus der Liste autorisierter Server.
4. Stellen Sie sicher, dass DHCP-Option 006 erreichbare DNS-Server verteilt.
5. Prüfen Sie Option 015 für das korrekte DNS-Suffix.

Autorisierte DHCP-Server können mit installiertem DHCP-PowerShell-Modul angezeigt werden:

```powershell
Get-DhcpServerInDC
```

Einen nicht mehr existierenden DHCP-Server nur dann aus der Autorisierung entfernen, wenn Name und IP-Adresse eindeutig stimmen:

```powershell
Remove-DhcpServerInDC -DnsName "DC01.contoso.local" -IPAddress 192.0.2.10
```

Ersetzen Sie Beispielname und Beispieladresse durch die tatsächlichen Werte.

# 15. Verweise in Gruppenrichtlinien und Skripten prüfen

Suchen Sie nach festen Verweisen auf den alten Server, zum Beispiel:

```text
\\DC01\Freigabe
LDAP://DC01
DC01.contoso.local
192.0.2.10
```

Prüfen Sie insbesondere:

- Anmeldeskripte
- Gruppenrichtlinien
- Laufwerkszuordnungen
- Druckerzuordnungen
- Geplante Aufgaben
- Dienstkonten und Dienste
- Monitoring
- Backupsoftware
- Anwendungen mit LDAP- oder LDAPS-Bindung
- Zertifikatvorlagen und Zertifizierungsstellen
- DFS-Namespace-Ziele

Verwenden Sie für Dienste möglichst den Domänennamen oder geeignete hochverfügbare Dienstnamen anstelle eines fest eingetragenen einzelnen Domänencontrollers.

# 16. Besonderheit bei einer Zertifizierungsstelle

War auf dem ausgefallenen Domänencontroller eine Active-Directory-Zertifizierungsstelle installiert, reicht die Domänencontroller-Metadatenbereinigung nicht aus.

In diesem Fall müssen zusätzlich berücksichtigt werden:

- Sicherung oder Wiederherstellung der CA-Datenbank
- Privater Schlüssel der Zertifizierungsstelle
- CA-Zertifikat
- Sperrlisten-Verteilungspunkte
- AIA-Veröffentlichung
- Zertifikatvorlagen
- Verweise in Active Directory
- Automatische Zertifikatsregistrierung
- OCSP-Konfiguration

Löschen Sie CA-Objekte nicht pauschal, wenn noch ausgestellte Zertifikate verwendet oder geprüft werden müssen.

# 17. Alten Domänencontroller niemals unverändert wieder einschalten

Nach einer erzwungenen Entfernung und Metadatenbereinigung gilt:

- Den alten Domänencontroller nicht wieder mit dem Produktivnetz verbinden.
- Keine alte virtuelle Maschine unverändert starten.
- Keinen alten Snapshot als Domänencontroller zurückrollen und produktiv verwenden.
- Keine parallele Installation mit demselben Computerkonto betreiben.

Wenn die Hardware oder virtuelle Maschine wiederverwendet werden soll:

1. Netzwerkverbindung zunächst trennen.
2. Betriebssystem neu installieren oder die Maschine sicher auf einen Zustand vor der Domänencontrollerrolle zurücksetzen.
3. Sicherstellen, dass keine alte Active-Directory-Datenbank mehr verwendet wird.
4. Einen eindeutigen Computernamen und eine korrekte IP-Konfiguration festlegen.
5. Den Server neu in die Domäne aufnehmen.
6. Bei Bedarf erneut ordnungsgemäß zum Domänencontroller heraufstufen.

# 18. Falls der alte Server wider Erwarten wieder verfügbar wird

Wenn die Metadatenbereinigung bereits durchgeführt oder FSMO-Rollen erzwungen übernommen wurden, darf der alte Domänencontroller nicht normal wieder in Betrieb genommen werden.

Die sichere Vorgehensweise ist:

1. Server vom Netzwerk getrennt halten.
2. Benötigte Nicht-AD-Daten nur über ein kontrolliertes Verfahren sichern.
3. Betriebssystem neu installieren.
4. Server erneut als Mitgliedsserver aufnehmen.
5. Bei Bedarf neu zum Domänencontroller heraufstufen.

# 19. Abschlusskontrolle

Die Bereinigung ist erfolgreich, wenn alle folgenden Punkte erfüllt sind:

- Der ausgefallene Server erscheint nicht mehr in der OU **Domain Controllers**.
- Er erscheint nicht mehr unter **Active Directory-Standorte und -Dienste**.
- `Get-ADDomainController -Filter *` zeigt ihn nicht mehr an.
- `netdom query fsmo` zeigt nur erreichbare FSMO-Rolleninhaber.
- DNS enthält keine relevanten A-, AAAA-, CNAME-, NS- oder SRV-Einträge des alten Servers.
- DHCP verteilt keine alte DNS-Serveradresse.
- Die verbleibenden Domänencontroller replizieren fehlerfrei.
- `dcdiag` meldet keine aktuellen kritischen Fehler bezüglich des entfernten Servers.
- SYSVOL und NETLOGON sind auf den verbleibenden Domänencontrollern verfügbar.
- Anwendungen verwenden keinen fest eingetragenen Verweis auf den entfernten Server.
- Der alte Domänencontroller bleibt dauerhaft offline oder wird vollständig neu installiert.

# 20. Kompakte Befehlsübersicht

```powershell
# Vorhandene Domänencontroller
Get-ADDomainController -Filter * |
    Select-Object HostName, Site, IPv4Address, IsGlobalCatalog, OperationMasterRoles

# FSMO-Rollen
Get-ADDomain | Select-Object PDCEmulator, RIDMaster, InfrastructureMaster
Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster

# Beispiel: Erzwungene Übernahme aller FSMO-Rollen
Move-ADDirectoryServerOperationMasterRole `
    -Identity "DC02" `
    -OperationMasterRole SchemaMaster,DomainNamingMaster,PDCEmulator,RIDMaster,InfrastructureMaster `
    -Force

# Kontrolle auf ein altes Computerobjekt
Get-ADComputer -Identity "DC01"

# Kontrolle auf ein altes Domänencontrollerobjekt
Get-ADDomainController -Identity "DC01"

# DNS-Konfiguration
Get-DnsClientServerAddress -AddressFamily IPv4
```

```cmd
netdom query fsmo
repadmin /replsummary
repadmin /showrepl *
repadmin /syncall /AdeP
dcdiag /e /c /v
dcdiag /test:dns /e /v
dcdiag /test:sysvolcheck /test:advertising
ipconfig /flushdns
ipconfig /registerdns
w32tm /query /status
w32tm /query /source
```

# Empfohlene Reihenfolge

1. Sicherstellen, dass mindestens ein funktionierender Domänencontroller vorhanden ist.
2. Replikation und DNS prüfen.
3. FSMO-Rollen ermitteln und gegebenenfalls übernehmen.
4. Abhängigkeiten wie DNS, DHCP, globaler Katalog, Zeitdienst und Zertifizierungsstelle prüfen.
5. Domänencontrollerobjekt über **Active Directory-Benutzer und -Computer** löschen.
6. **Active Directory-Standorte und -Dienste** auf Reste prüfen.
7. Falls erforderlich, `ntdsutil` zur Metadatenbereinigung verwenden.
8. DNS-, DHCP- und Anwendungsverweise bereinigen.
9. Replikation anstoßen.
10. `repadmin`, `dcdiag`, DNS und Ereignisprotokolle kontrollieren.
11. Alte Maschine dauerhaft offline lassen oder vollständig neu installieren.