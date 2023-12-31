# VDI Verbindungstrennung bei Gruppenrichtlinienübernahme

Verbindungtrennung der VDI Remotesitzung bei aktualisierung der Gruppenrichtlinie

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

Auf dem Server folgenden Registryschlüssel abändern

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\fDenyTSConnections
```

Den Wert von Standard "1" auf "0" setzen. Dies muss bei jedem einzelnen VDI Desktop auch durchgeführt werden, da hier jeder unabhängig von dem anderen agiert.

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
Wird nachgereicht
```