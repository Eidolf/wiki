# DPM Backupjobs nacheinander abarbeiten

Wenn das SAN keinen Hardware VSS Provider unterstützt kann man dem DPM eine Richtlinie vorgeben das er den MS VSS Software Provider verwendet und dadurch Backupjobs einzeln nacheinander abarbeitet und nicht gleichzeitig versucht zu schreiben.  
  
Quelle:

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/ff634192.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/ff634192.aspx</a> (DPM 2010)
<a class="external free" href="http://technet.microsoft.com/de-de/library/hh757922.aspx" rel="nofollow">http://technet.microsoft.com/de-de/library/hh757922.aspx</a> (DPM 2012)
```