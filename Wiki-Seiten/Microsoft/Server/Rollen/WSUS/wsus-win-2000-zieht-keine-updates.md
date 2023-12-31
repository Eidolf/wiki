# WSUS Win 2000 zieht keine Updates

An Windows 2000 Clients werden keine WSUS Updates mehr gezogen, in der Ereignisanzeige wird keine Fehlermeldung angezeigt die zur Lösung beitragen kann. Im WindowsUpdate.log unter "C:\\WindowsNT\\WindowsUpdate.log" wird folgendes angezeigt:

```
2011-11-23 10:22:46:750 1036 74c Misc Validating signature for C:\WINNT\SoftwareDistribution\Download\5eec8c5b386a4412753bdd033b3363ee\650a4537454d6e51e143883502047d29b328bcd6:
2011-11-23 10:22:46:797 1036 74c Misc WARNING: Error: 0x800b0109 when verifying trust for C:\WINNT\SoftwareDistribution\Download\5eec8c5b386a4412753bdd033b3363ee\650a4537454d6e51e143883502047d29b328bcd6
2011-11-23 10:22:46:797 1036 74c Misc WARNING: Digital Signatures on file C:\WINNT\SoftwareDistribution\Download\5eec8c5b386a4412753bdd033b3363ee\650a4537454d6e51e143883502047d29b328bcd6 are not trusted: Error 0x800b0109
2011-11-23 10:22:46:797 1036 74c DnldMgr WARNING: File failed postprocessing, error = 800b0109
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-erkl%C3%A4rung%3A-1">Erklärung:</span>

Microsoft hat eine neue Root Zertifikat herausgebracht, das nicht mehr mit Windows Updates, an die Win2000 Clients verteilt wurde und deshalb auch keine Verbindung mehr mit WSUS hergestellt werden kann.

# <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

Das neue Root Zertifikat "Microsoft Root Certificate Authority" mit dem Ablaufdatum vom 10.05.2021 als vertrauenswürdiges Stammzertifikat installieren. Es kann von jedem anderen Rechner exportiert werden.

Weiterhin können noch folgende DLL Dateien neu registriert werden, wenn das Zertifikat schon vorhanden ist.

```
Regsvr32 WINTRUST.DLL
Regsvr32 INITPKI.DLL
Regsvr32 DSSENH.DLL
Regsvr32 RSAENH.DLL 
Regsvr32 Gpkcsp.dll
Regsvr32 Sccbase.dll
Regsvr32 Slbcsp.dll
Regsvr32 CRYPTDLG.DLL
Regsvr32 Mssip32.dll
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/Forefrontclientgeneral/thread/7fcae7e1-85c7-43d9-8193-0ad10c4161e0" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/Forefrontclientgeneral/thread/7fcae7e1-85c7-43d9-8193-0ad10c4161e0</a>
```