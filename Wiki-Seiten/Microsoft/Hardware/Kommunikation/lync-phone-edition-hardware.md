# Lync Phone Edition Hardware

# <span class="mw-headline" id="bkmrk-polycom-1">Polycom</span>

## <span class="mw-headline" id="bkmrk-remote-access-log-1">Remote Access Log</span>

<div class="vector-body" id="bkmrk-am-telefon-selbst-da"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Am Telefon selbst das **Remote Access Log** aktivieren.
2. Über eine FTP Verbindung auf die IP Adresse mit dem IE zugreifen `<a class="external free" href="ftp://xxx.xxx.xxx.xxx/" rel="nofollow">ftp://xxx.xxx.xxx.xxx</a>` oder einen FTP Client dazu verwenden.
3. Die Datei `system.clg1` herunterladen (Name variiert eventuell).
4. Zum umwandeln in ein lesbares Format wird **ReadLog** verwendet &gt; [Download hier](https://info.eidolf.de/speicher_eidolf/public/readlog)
5. **ReadLog.zip** entpacken in einen Ordner mit der **system.clg1**
6. Ein CMD Fenster öffnen und zum Pfad der beiden Dateien wechseln danach folgenden Befehl eingeben `readlog.exe "system1.clg1" "system1.txt"`
7. Die neue **system1.txt** kann mit einem Editor gelesen und analysiert werden.

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://jackstromberg.com/2013/09/how-do-i-analyze-log-files-off-polycom-phones/" rel="nofollow">https://jackstromberg.com/2013/09/how-do-i-analyze-log-files-off-polycom-phones/</a>
```