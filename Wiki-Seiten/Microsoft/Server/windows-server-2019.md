# Windows Server 2019

# <span class="mw-headline" id="bkmrk-rollen-1">Rollen</span>

## <span class="mw-headline" id="bkmrk-storage-migration-se-1">Storage Migration Service</span>

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/en-us/windows-server/storage/storage-migration-service/overview" rel="nofollow">https://docs.microsoft.com/en-us/windows-server/storage/storage-migration-service/overview</a>
```

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-wiederkehrende-meldu-1">Wiederkehrende Meldung nach unsachgemäßen herunterfahren</span>

Die Meldung nach einem Crash erscheint immer wieder, obwohl jedes Mal mit einer Beschreibung bestätigt wird.  
Wie in der Quelle zu lesen soll es angeblich funktionieren sich mit dem lokalen Administrator anzumelden, hier zu bestätigen und danach einen Neustart durchführen.  
Die Lösung mit etwas weniger Aufwand steht auch im gleichen Thread, nämlich ein paar Registry Einträge zu löschen.  
Unter dem Pfad  
`Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Reliability`  
Folgende Einträge löschen  
**DirtyShutdown**  
**DirtyShutdownTime**

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://social.technet.microsoft.com/Forums/windowsserver/en-US/c543eed6-f647-4563-85ce-8e72d1b36a85/shutdown-event-tracker-keeps-appearing-after-installing-kb4490481-and-an-unexpected-shutdown?forum=ws2019" rel="nofollow">https://social.technet.microsoft.com/Forums/windowsserver/en-US/c543eed6-f647-4563-85ce-8e72d1b36a85/shutdown-event-tracker-keeps-appearing-after-installing-kb4490481-and-an-unexpected-shutdown?forum=ws2019</a>
```