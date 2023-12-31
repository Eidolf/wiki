# Antivirus Scan Ausnahmen

# <span class="mw-headline" id="bkmrk-allgemeine-ausnahmen-1">Allgemeine Ausnahmen</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=1 "Abschnitt bearbeiten: Allgemeine Ausnahmen")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-%25systemroot%25%5Csystem3"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. %systemroot%\\System32\\Spool (and all the sub-folders and files)
2. %systemroot%\\SoftwareDistribution\\Datastore

</div></div></div># <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-dc-%2F-dns-%2F-dhcp-ausn-1">DC / DNS / DHCP Ausnahmen</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=2 "Abschnitt bearbeiten: DC / DNS / DHCP Ausnahmen")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-%25systemroot%25%5Csysvol-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. %systemroot%\\Sysvol folder (include all the sub-folders and files)
2. %systemroot%\\system32\\dhcp folder (include all the sub-folders and files)
3. %systemroot%\\system32\\dns folder (include all the sub-folders and files)
4. %systemroot%\\ntds

</div></div></div># <span class="mw-headline" id="bkmrk-exchange-2010">Exchange 2010</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=3 "Abschnitt bearbeiten: Exchange 2010")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-%25exchangeinstallpath"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. %ExchangeInstallPath%\\Mailbox
2. %ExchangeInstallPath%\\GroupMetrics
3. %ExchangeInstallPath%\\TransportRoles\\Logs
4. %ExchangeInstallPath%\\Logging
5. %ExchangeInstallPath%\\ExchangeOAB
6. %SystemRoot%\\System32\\Inetsrv
7. %ExchangeInstallPath%\\Mailbox\\MDBTEMP
8. %ExchangeInstallPath%\\TransportRoles\\Logs
9. %ExchangeInstallPath%\\TransportRoles
10. %ExchangeInstallPath%\\TransportRoles\\Data\\Queue
11. %ExchangeInstallPath%\\TransportRoles\\Data\\SenderReputation
12. %ExchangeInstallPath%\\TransportRoles\\Data\\IpFilter
13. %ExchangeInstallPath%\\Working\\OleConvertor
14. %SystemDrive%\\inetpub\\temp\\IIS Temporary Compressed Files
15. %systemroot%\\IIS Temporary Compressed Files
16. %SystemRoot%\\System32\\Inetsrv
17. %ExchangeInstallPath%\\ClientAccess
18. %ExchangeInstallPath%\\Logging\\POP3
19. %ExchangeInstallPath%\\Logging\\IMAP4
20. %ExchangeInstallPath%\\Working\\OleConvertor

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=4 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/bb332342.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/bb332342.aspx</a>
```

# <span class="mw-headline" id="bkmrk-hyper-v">Hyper-V</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=5 "Abschnitt bearbeiten: Hyper-V")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-vm-hauptordner"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. VM Hauptordner

</div></div></div>Dateien:

<div class="vector-body" id="bkmrk-vhd-vsv-iso-avhd-vfd"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. VHD
2. VSV
3. ISO
4. AVHD
5. VFD
6. XML

</div></div></div>Prozesse:

<div class="vector-body" id="bkmrk-vmms.exe-vmwp.exe-vm"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. VMMS.EXE
2. VMWP.EXE
3. VMSWP.EXE

</div></div></div># <span class="mw-headline" id="bkmrk-symantec-backup-exec-1">Symantec Backup Exec</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=6 "Abschnitt bearbeiten: Symantec Backup Exec")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-backup-exec-server-s-1">Backup Exec Server-side FCS exclusions</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=7 "Abschnitt bearbeiten: Backup Exec Server-side FCS exclusions")<span class="mw-editsection-bracket">\]</span></span>

Process Exclusions

<div class="vector-body" id="bkmrk-c%3A%5Cprogram-files%5Csym"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- C:\\Program Files\\Symantec\\Backup Exec\\bengine.exe
- C:\\Program Files\\Symantec\\Backup Exec\\beserver.exe
- C:\\Program Files\\Symantec\\Backup Exec\\benetns.exe
- C:\\Program Files\\Symantec\\Backup Exec\\BkupExec.exe
- C:\\Program Files\\Symantec\\Backup Exec\\beremote.exe
- C:\\Program Files\\Microsoft SQL Server\\MSSQL.1\\MSSQL\\Binn\\sqlservr.exe
- C:\\Program Files\\Microsoft SQL Server\\90\\Shared\\sqlbrowser.exe
- C:\\Program Files\\Symantec\\Backup Exec\\pvlsvr.exe
- C:\\WINDOWS\\System32\\vssvc.exe (VSS service)

</div></div></div>  
Do not scan the following extensions

<div class="vector-body" id="bkmrk-db-db%24-chk"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- DB
- DB$
- CHK

</div></div></div>Folder exclusion:

<div class="vector-body" id="bkmrk-c%3A%5Cprogram-files%5Csym-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- C:\\Program Files\\Symantec\\Backup Exec

</div></div></div>## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-backup-exec-client-s-1">Backup Exec client-side (File Server) FCS exclusions</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Scan_Ausnahmen&action=edit&section=8 "Abschnitt bearbeiten: Backup Exec client-side (File Server) FCS exclusions")<span class="mw-editsection-bracket">\]</span></span>

Process Exclusions:

<div class="vector-body" id="bkmrk-c%3A%5Cprogram-files%5Csym-2"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- C:\\Program Files\\Symantec\\Backup Exec\\beremote.exe
- C:\\WINDOWS\\System32\\vssvc.exe (VSS service)

</div></div></div>Do not scan the following extensions

<div class="vector-body" id="bkmrk-db-db%24-chk-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- DB
- DB$
- CHK

</div></div></div>Folder exclusion:

<div class="vector-body" id="bkmrk-c%3A%5Cprogram-files%5Csym-3"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- C:\\Program Files\\Symantec\\Backup

</div></div></div>  
Quelle:

```
<a class="external free" href="http://myitforum.com/cs2/blogs/scassells/archive/2007/05/14/what-anti-virus-scanning-exclusions-should-be-considered-for-system-and-servers.aspx" rel="nofollow">http://myitforum.com/cs2/blogs/scassells/archive/2007/05/14/what-anti-virus-scanning-exclusions-should-be-considered-for-system-and-servers.aspx</a>
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/Forefrontclientsetup/thread/c93990f3-6dcd-4a83-ad3c-8eeb3006454d" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/Forefrontclientsetup/thread/c93990f3-6dcd-4a83-ad3c-8eeb3006454d</a>
```