# Powershell

## <span class="mw-headline" id="bkmrk-skript-signieren-1">Skript signieren</span>

Voraussetzung ein Script zu signieren ist ein gültiges Codesignierungszertifikat im Benutzer Zertifikatsspeicher.

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-pr%C3%BCfen-ob-codesignie-1">Prüfen ob Codesignierungszertifikat vorhanden ist</span>

```
Get-ChildItem cert:\CurrentUser\My -CodeSigningCert
```

### <span class="mw-headline" id="bkmrk-skript-signieren-3">Skript Signieren</span>

Folgender Signiervorgang wird in einer Umgebung mit Root CA bevorzugt, **Achtung:** das ist kein direktes ps1 Script, die Befehle werden nacheinander eingegeben.

```
$Filepath = read-host
> "Pfad zu Skript angeben" (ohne Anführungszeichen)
$Cert = (Get-ChildItem Cert:\CurrentUser\my -CodeSigningCert)[0]
Set-AuthenticodeSignature -FilePath "$Filepath" -Certificate $Cert -IncludeChain "All" -TimeStampServer "http://timestamp.fabrikam.com/scripts/timstamper.dll"
```

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Set-AuthenticodeSignature" rel="nofollow">https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Set-AuthenticodeSignature</a>
<a class="external free" href="http://www.hanselman.com/blog/SigningPowerShellScripts.aspx" rel="nofollow">http://www.hanselman.com/blog/SigningPowerShellScripts.aspx</a>
```

## <span class="mw-headline" id="bkmrk-dateien-umbenennen-1">Dateien umbenennen</span>

### <span class="mw-headline" id="bkmrk-mittleren-teil-umben-1">Mittleren Teil umbenennen</span>

Wenn in einem Beispieldateinamen **( Test-01.test )** das **( -0 )** in **( 01x )** geändert werden soll muss man folgenden Befehl ausführen.  
Hinweis:  
In folgendem Befehl wird **"dir"** zur Auflistung alle Dateien in einem Ordner verwendet, wenn man die Auswahl eingrenzen will, kann man z.B. " dir \*.jpg" verwenden, oder einen anderen Befehl wie z.B. "get-childitem"

```
dir | foreach { move-item -literal $_ $_.name.replace("st-0","st01x")}
```

## <span class="mw-headline" id="bkmrk-dateien-verschieben-1">Dateien verschieben</span>

### <span class="mw-headline" id="bkmrk-dateien-von-pfad-x-r-1">Dateien von Pfad X Rekursive nach Pfad Y</span>

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

## <span class="mw-headline" id="bkmrk-prozess-auf-remote-r-1">Prozess auf Remote-Rechner erzwungen beenden</span>

1. PowerShell öffnen
2. Dienst ID herausfinden mit folgenden Befehl für z.B. den Hyper-V Verwaltungsdienst 
    - `Get-Process -ComputerName Remotename -name vmms`
3. Den Prozess erzwungen beenden mit zuvor herausgefundener ID 
    - `Invoke-Command -ComputerName w2012-clu-n1 -ScriptBlock {Stop-Process -ID 3096}`

## <span class="mw-headline" id="bkmrk-powershell-web-acces-1">PowerShell Web Access</span>

Hiermit kann man eine Powershell Konsole im Browser öffnen und sich hiermit auf verschiedene Server verbinden.

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://blog.pluralsight.com/configure-powershell-web-access" rel="nofollow">http://blog.pluralsight.com/configure-powershell-web-access</a>
<a class="external free" href="http://technet.microsoft.com/en-us/library/hh831611.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/hh831611.aspx</a>
```

## <span class="mw-headline" id="bkmrk-installation-von-v6-1">Installation von v6</span>

```
iex "& { $(irm https://aka.ms/install-powershell.ps1) } -UseMSI"
Install-PackageProvider Nuget –Force
Install-Module –Name PowerShellGet –Force
Exit
```

### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://www.thomasmaurer.ch/2019/03/how-to-install-and-update-powershell-6/" rel="nofollow">https://www.thomasmaurer.ch/2019/03/how-to-install-and-update-powershell-6/</a>
<a class="external free" href="https://www.thomasmaurer.ch/2019/02/update-powershellget-and-packagemanagement/" rel="nofollow">https://www.thomasmaurer.ch/2019/02/update-powershellget-and-packagemanagement/</a>
```