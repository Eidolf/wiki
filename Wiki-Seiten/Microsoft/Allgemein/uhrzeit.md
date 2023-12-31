# Uhrzeit

# <span class="mw-headline" id="bkmrk-zeitsynchronisierung-1">Zeitsynchronisierung</span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-regeintrag-f%C3%BCr-w32ti-1">Regeintrag für w32time</span>

```
HKEY_LOCAL_MACHINE\ SYSTEM\ CurrentControlSet\ Services\ W32Time\ Parameters
```

<div class="vector-body" id="bkmrk-parameter-wertname-%C2%A0"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output"><table border="1" class="wikitable"><caption>Parameter</caption><tbody><tr><th>Wertname</th><th> </th><th>Datentyp</th><th> </th><th>Beschreibung !</th></tr><tr><td>Type</td><td> </td><td>REG\_SZ</td><td> </td><td>Wird verwendet, um zu steuern, wie ein Computer synchronisiert wird. Nt5DS = Synchronisierung nach Domänenhierarchie (Standard)

NTP = Synchronisierung nach manuell konfigurierter Quelle

NoSync = Keine Synchronisierung der Zeit

Bei dem Wert "Nt5DS" kann keine manuell konfigurierte Quelle verwendet werden.

</td></tr></tbody></table>

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.winfaq.de/faq_html/Content/tip1500/onlinefaq.php?h=tip1749.htm" rel="nofollow">http://www.winfaq.de/faq_html/Content/tip1500/onlinefaq.php?h=tip1749.htm</a>
```

## <span class="mw-headline" id="bkmrk-pdc-zeitgeber-manuel-1">PDC Zeitgeber manuell eingeben</span>

Um einen PDC seinen Zeitgeber Manuell zuzuweisen folgendes eingeben:

```
w32tm /config /manualpeerlist:"peer1, peer2" /syncfromflags:manual /reliable:yes /update
```

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/cc786897(WS.10).aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/cc786897(WS.10).aspx</a>
<a class="external free" href="http://technet.microsoft.com/en-us/library/cc773013(WS.10).aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/cc773013(WS.10).aspx</a>
<a class="external free" href="http://blog.kopfteam.de/timeserver-sauber-konfigurieren-neue-methode/" rel="nofollow">http://blog.kopfteam.de/timeserver-sauber-konfigurieren-neue-methode/</a>
```

# <span class="mw-headline" id="bkmrk-zeitdienstprobleme-1">Zeitdienstprobleme</span>

Wenn der net start w32time nicht funktioniert da eine Fehlermeldung angezeigt wird das er eine Dateien nicht finden kann. Als erstes w32tm neu registrieren.

<div class="vector-body" id="bkmrk-w32tm-%2Funregister-w3"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- w32tm /unregister
- w32tm /register

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="http://blogs.technet.com/b/industry_insiders/archive/2006/08/29/w32-tm-service.aspx" rel="nofollow">http://blogs.technet.com/b/industry_insiders/archive/2006/08/29/w32-tm-service.aspx</a>
```