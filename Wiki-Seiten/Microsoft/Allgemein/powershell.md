---
title: Powershell
description: 
published: true
date: 2025-08-14T10:32:13.460Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:29:12.851Z
---

# Skript signieren

Voraussetzung ein Script zu signieren ist ein gültiges Codesignierungszertifikat im Benutzer Zertifikatsspeicher.

## Prüfen ob Codesignierungszertifikat vorhanden ist

`Get-ChildItem cert:\CurrentUser\My -CodeSigningCert`

## Skript Signieren

Folgender Signiervorgang wird in einer Umgebung mit Root CA bevorzugt, **Achtung:** das ist kein direktes ps1 Script, die Befehle werden nacheinander eingegeben.

```
$Filepath = read-host
> "Pfad zu Skript angeben" (ohne Anführungszeichen)
$Cert = (Get-ChildItem Cert:\CurrentUser\my -CodeSigningCert)[0]
Set-AuthenticodeSignature -FilePath "$Filepath" -Certificate $Cert -IncludeChain "All" -TimeStampServer "http://timestamp.fabrikam.com/scripts/timstamper.dll"
```

## Quelle:

https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Set-AuthenticodeSignature
http://www.hanselman.com/blog/SigningPowerShellScripts.aspx

# Dateien umbenennen

## Mittleren Teil umbenennen

Wenn in einem Beispieldateinamen **( Test-01.test )** das **( -0 )** in **( 01x )** geändert werden soll muss man folgenden Befehl ausführen.  
Hinweis:  
In folgendem Befehl wird **"dir"** zur Auflistung alle Dateien in einem Ordner verwendet, wenn man die Auswahl eingrenzen will, kann man z.B. " dir \*.jpg" verwenden, oder einen anderen Befehl wie z.B. "get-childitem"

`dir | foreach { move-item -literal $_ $_.name.replace("st-0","st01x")}`

# Dateien verschieben

## Dateien von Pfad X Rekursive nach Pfad Y

Folgend ein kleines Skript das Dateien aus einem angegebenen Pfad rekursive sucht und diese anschließend an einen anderen Pfad verschiebt. Vor dem verschieben werden nochmal zur Bestätigung die Namen der gefundenen Dateien ausgegeben.  
Einfach als **".ps1"** abspeichern.

```
$VonPfad = Read-Host "Bitte Pfad angeben von wo aus Rekursiv Dateien gesucht werden sollen"
$NachPfad = Read-Host "Bitte Pfad angeben wohin die gesuchten Dateien verschoben werden sollen"
$DateiName = Read-Host "Bitte Dateinamen angeben der zutrifft z.B. (DateiXY_*_TXT.docx)"
cd $VonPfad
$DateiAnzeige = Get-ChildItem -Recurse -File $DateiName
Write-Host $DateiAnzeige.Name
Write-Host "Sollen folgende Dateien verschoben werden dann bitte Enter drücken"
Pause
Move-Item -Path $DateiAnzeige -Destination $NachPfad
```

# Prozess auf Remote-Rechner erzwungen beenden

1. PowerShell öffnen
2. Dienst ID herausfinden mit folgenden Befehl für z.B. den Hyper-V Verwaltungsdienst 
    - `Get-Process -ComputerName Remotename -name vmms`
3. Den Prozess erzwungen beenden mit zuvor herausgefundener ID 
    - `Invoke-Command -ComputerName w2012-clu-n1 -ScriptBlock {Stop-Process -ID 3096}`

# PowerShell Web Access

Hiermit kann man eine Powershell Konsole im Browser öffnen und sich hiermit auf verschiedene Server verbinden.

## Quelle:

http://blog.pluralsight.com/configure-powershell-web-access
http://technet.microsoft.com/en-us/library/hh831611.aspx

# Installation von v6

```
iex "& { $(irm https://aka.ms/install-powershell.ps1) } -UseMSI"
Install-PackageProvider Nuget –Force
Install-Module –Name PowerShellGet –Force
Exit
```

## Quelle:
https://www.thomasmaurer.ch/2019/03/how-to-install-and-update-powershell-6/
https://www.thomasmaurer.ch/2019/02/update-powershellget-and-packagemanagement/
