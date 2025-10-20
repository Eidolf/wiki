---
title: rebuild-des-wmi-repository
description: 
published: true
date: 2023-12-31T14:28:54.140Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:28:51.134Z
---

# Rebuild des WMI Repository

# <span class="mw-headline" id="bkmrk-beschreibung-1">Beschreibung</span>

**Winmgmt /salvagerepository** Note this command will take the content of the inconsistent repository and merge it into the rebuilt repository if it is readable

If the above doesn’t work, then run:

**Winmgmt /resetrepository** Note this will reset repository to the initial state when the OS was first installed

For Windows XP and Windows Server 2003, there are no built in switches to rebuild the Repository, so you must do it manually.

Warning: Rebuilding the WMI repository has resulted in some 3rd party products not working until their setup is re-run &amp; their MOF re-added back to the repository.

  
If /salvagerepository or /resetrepository does not resolve the issue, then manually rebuild repository:

<div class="vector-body" id="bkmrk-change-startup-type-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Change startup type to Window Management Instrumentation (WMI) Service to disabled
2. Stop the WMI Service; you may need to stop IP Helper Service first or other dependent services before it allows you to stop WMI Service
3. Rename the repository folder: C:\\WINDOWS\\system32\\wbem\\Repository to Repository.old
4. Open a CMD Prompt with elevated privileges
5. CD windows\\system32\\wbem
6. for /f %%s in ('dir /b /s \*.dll') do regsvr32 /s %%s
7. Set the WMI Service type back to Automatic and start WMI Service
8. cd /d c:\\ ((go to the root of the c drive, this is important))
9. for /f %%s in ('dir /s /b \*.mof \*.mfl') do mofcomp %%s
10. Reboot the Server

</div></div></div>## <span class="mw-headline" id="bkmrk-skripte%3A">Skripte:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Rebuild_des_WMI_Repository&action=edit&section=2 "Abschnitt bearbeiten: Skripte:")<span class="mw-editsection-bracket">\]</span></span>

Standard:

```
net stop ccmexec
net stop winmgmt
 %SYSTEMDRIVE%
 CD %windir%\system32\wbem
 rd /S /Q repository
 net start winmgmt
 net start ccmexec
 for /f %%s in ('dir /b /s *.dll') do regsvr32 /s %%s
 for /f %%s in ('dir /b *.mof') do mofcomp %%s
 wmiprvse /regserver
```

Ausführlich:

```
sc config winmgmt start= disabled
net stop winmgmt /y
%systemdrive%
cd %windir%\system32\wbem
for /f %%s in ('dir /b *.dll') do regsvr32 /s %%s
for /f %%s in ('dir /b *.exe') do regsvr32 /s %%s
cd %windir%\sysWOW64\wbem
for /f %%s in ('dir /b *.dll') do regsvr32 /s %%s
for /f %%s in ('dir /b *.exe') do regsvr32 /s %%s
cd %windir%\system32\wbem
wmiprvse /regserver 
winmgmt /regserver 
sc config winmgmt start= auto
net start winmgmt
cd %windir%\system32\wbem
for /f %%s in ('dir /s /b *.mof *.mfl') do mofcomp %%s
cd %windir%\sysWOW64\wbem
for /f %%s in ('dir /s /b *.mof *.mfl') do mofcomp %%s
pause
```

## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Rebuild_des_WMI_Repository&action=edit&section=3 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://blogs.technet.com/b/askperf/archive/2009/04/13/wmi-rebuilding-the-wmi-repository.aspx#pi136298=1" rel="nofollow">http://blogs.technet.com/b/askperf/archive/2009/04/13/wmi-rebuilding-the-wmi-repository.aspx#pi136298=1</a>
```