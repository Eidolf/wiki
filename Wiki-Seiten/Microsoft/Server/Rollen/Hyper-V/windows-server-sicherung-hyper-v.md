---
title: windows-server-sicherung-hyper-v
description: 
published: true
date: 2023-12-31T14:34:59.905Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:56.686Z
---

# Windows Server Sicherung Hyper-V

# <span class="mw-headline" id="bkmrk-beschreibung">Beschreibung</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Windows_Server_Sicherung_Hyper-V&action=edit&section=1 "Abschnitt bearbeiten: Beschreibung")<span class="mw-editsection-bracket">\]</span></span>

Standardmäßig kann die Windows Server Sicherung keinen Hyper-V sichern, hierzu muss erst der Hyper-V VSS Provider dem Sicherungsfeature bereitgestellt werden.

# <span class="mw-headline" id="bkmrk-batchdateien">Batchdateien</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Windows_Server_Sicherung_Hyper-V&action=edit&section=2 "Abschnitt bearbeiten: Batchdateien")<span class="mw-editsection-bracket">\]</span></span>

Folgenden Codes kann man in eine Batchdatei schreiben um erstens zu prüfen ob der Hyper-V Regeintrag für die Windows Server Sicherung vorhanden ist und wenn nicht, diesen mit der zweiten Batchdatei hinzuzufügen.

## <span class="mw-headline" id="bkmrk-hyperv-vss-check.bat-1">HyperV-VSS-Check.bat</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Windows_Server_Sicherung_Hyper-V&action=edit&section=3 "Abschnitt bearbeiten: HyperV-VSS-Check.bat")<span class="mw-editsection-bracket">\]</span></span>

```
reg query "HKLM\Software\Microsoft\Windows nt\currentversion\WindowsServerBackup\Application Support\{66841CD4-6DED-4F4B-8F17-FD23F8DDC3DE}" /s
```

## <span class="mw-headline" id="bkmrk-hyperv-vss.bat">HyperV-VSS.bat</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Windows_Server_Sicherung_Hyper-V&action=edit&section=4 "Abschnitt bearbeiten: HyperV-VSS.bat")<span class="mw-editsection-bracket">\]</span></span>

```
reg add "HKLM\Software\Microsoft\Windows nt\currentversion\WindowsServerBackup\Application Support\{66841CD4-6DED-4F4B-8F17-FD23F8DDC3DE}"
reg add "HKLM\Software\Microsoft\Windows nt\currentversion\WindowsServerBackup\Application Support\{66841CD4-6DED-4F4B-8F17-FD23F8DDC3DE}" /v "Application Identifier" /t REG_SZ /d Hyper-V
```

# <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Windows_Server_Sicherung_Hyper-V&action=edit&section=5 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/video/how-do-i-backup-hyper-v-with-windows-server-backup.aspx" rel="nofollow">http://technet.microsoft.com/en-us/video/how-do-i-backup-hyper-v-with-windows-server-backup.aspx</a>
```