# Exchange Offlineadressbuch

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-exchange-shell-befeh-1">Exchange Shell Befehl für aktualisierung des Offline Adressbuchs</span>

## <span class="mw-headline" id="bkmrk-nur-standard-offline-1">Nur Standard Offline Adressbuch</span>

```
Get-OfflineAddressBook | Update-OfflineAddressBook -vb
```

## <span class="mw-headline" id="bkmrk-bestimmtes-offline-a-1">Bestimmtes Offline Adressbuch</span>

```
Update-OfflineAdressBook -identity "Offline Adressbuch"
```

# <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-exchange-logging-inh-1">Exchange Logging Inhalt erhöhen</span>

## <span class="mw-headline" id="bkmrk-konsole-1">Konsole</span>

<div class="vector-body" id="bkmrk-in-exchange-system-m"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. In Exchange System Manager, double-click Servers, right-click a server, and then click Properties.
2. On the Diagnostic Logging tab, under Services, select MSExchangeSA.
3. In Categories, click OAL Generator.
4. In Logging level, select Maximum.

</div></div></div>## <span class="mw-headline" id="bkmrk-shell-1">Shell</span>

```
Get-EventLogLevel
```

```
Set-EventLogLevel -Identity "MSExchangeAL\Account Management" -Level High
Set-EventLogLevel -Identity "MSExchangeSA\OAL Generator" -Level High
```

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-%C3%9Cbersicht-der-loglev-1">Übersicht der Loglevels</span>

<div class="vector-body" id="bkmrk-parameter-logging-le"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output"><table border="1" class="wikitable"><caption>Parameter</caption><tbody><tr><th>Logging level</th><th>Description !</th></tr><tr><td>Lowest</td><td>Only critical events, error events, and events with a logging level of zero (0) are logged. Note: This is the default level for all services on Exchange servers.

</td></tr><tr><td>Low</td><td>Events with a logging level of 1 or lower are logged.</td></tr><tr><td>Medium</td><td>Events with a logging level of 3 or lower are logged.</td></tr><tr><td>High</td><td>Events with a logging level of 5 or lower are logged.</td></tr><tr><td>Expert</td><td>Events with a logging level of 7 or lower are logged.</td></tr></tbody></table>

</div></div></div># <span class="mw-headline" id="bkmrk-tool-zum-testen-1">Tool zum testen</span>

OABInteg (Offline Address Book Integrity Checker) zum Prüfen des Offline Adressbuchs  
Download [OABInteg.zip.rar](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=OABInteg.zip.rar "OABInteg.zip.rar") (.rar entfernen)

# <span class="mw-headline" id="bkmrk-quellen%3A-1">Quellen:</span>

```
<a class="external free" href="http://www.sysadmin-forum.de/blogs/6-exchange-2010-fehler-0x80004005-und-offline-adressbuch-neu-erstellen.html" rel="nofollow">http://www.sysadmin-forum.de/blogs/6-exchange-2010-fehler-0x80004005-und-offline-adressbuch-neu-erstellen.html</a>
<a class="external free" href="http://blogs.msdn.com/b/dgoldman/archive/2007/10/10/how-to-tell-what-public-folder-store-your-outlook-client-is-going-to-when-you-fail-to-download-the-oab-with-error-0x8004010f.aspx" rel="nofollow">http://blogs.msdn.com/b/dgoldman/archive/2007/10/10/how-to-tell-what-public-folder-store-your-outlook-client-is-going-to-when-you-fail-to-download-the-oab-with-error-0x8004010f.aspx</a>
<a class="external free" href="http://blogs.msdn.com/b/dgoldman/archive/2006/11/27/error-0x80190194-when-using-an-outlook-2007-client-to-download-a-web-distribution-enabled-oab.aspx" rel="nofollow">http://blogs.msdn.com/b/dgoldman/archive/2006/11/27/error-0x80190194-when-using-an-outlook-2007-client-to-download-a-web-distribution-enabled-oab.aspx</a>
<a class="external free" href="http://www.msxfaq.de/e2007/oabgen2007.htm" rel="nofollow">http://www.msxfaq.de/e2007/oabgen2007.htm</a>
<a class="external free" href="http://forums.msexchange.org/m_1800455013/mpage_3/tm.htm" rel="nofollow">http://forums.msexchange.org/m_1800455013/mpage_3/tm.htm</a>
<a class="external free" href="http://www.administrator.de/index.php?content=144334" rel="nofollow">http://www.administrator.de/index.php?content=144334</a>
```