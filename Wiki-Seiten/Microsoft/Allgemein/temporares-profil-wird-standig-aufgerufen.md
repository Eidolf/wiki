# Temporäres Profil wird ständig aufgerufen

Wenn kein Benutzerordner mehr vorhanden ist, die Sicherheitseinstellung des Server Ordners überprüft wurden und keine System Dateien (Temp Dateien) im Server gespeicherten Profil vorhanden sind. Dann am Client das Profil aus der Registry löschen.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\ProfileList\S-1-5-21....
```

  
  
Quelle:

```
<a class="external free" href="http://blog.tech4him.com/2009/09/windows-7-and-the-dreaded-temp-profile/" rel="nofollow">http://blog.tech4him.com/2009/09/windows-7-and-the-dreaded-temp-profile/</a>
```