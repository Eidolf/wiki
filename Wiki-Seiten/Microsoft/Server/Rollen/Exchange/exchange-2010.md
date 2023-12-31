# Exchange 2010

## <span class="mw-headline" id="bkmrk-autodiscover-1">Autodiscover</span>

[Autodiscover Exchange 2010](https://wiki.eidolf.de/index.php/Autodiscover_Exchange_2010 "Autodiscover Exchange 2010")

## <span class="mw-headline" id="bkmrk-forfront-protection--1">Forfront Protection for Exchange</span>

[Cloudmark Antispam keine Aktualisierung](https://wiki.eidolf.de/index.php/Cloudmark_Antispam_keine_Aktualisierung "Cloudmark Antispam keine Aktualisierung")

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-exchange-de%2Faktivier-1">Exchange De/Aktivieren von Mailboxen</span>

### <span class="mw-headline" id="bkmrk-schnelles-wiederverb-1">Schnelles Wiederverbinden einer Deaktivierten Mailbox</span>

#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://russburden.wordpress.com/2012/08/21/exchange-2010-disable-and-connect-mailboxes/" rel="nofollow">http://russburden.wordpress.com/2012/08/21/exchange-2010-disable-and-connect-mailboxes/</a>
```

## <span class="mw-headline" id="bkmrk-exchange-deinstallat-1">Exchange Deinstallation</span>

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-deinstallation-des-l-1">Deinstallation des letztens Servers in einer Domäne</span>

Im Fall das ein Exchange Server der letzte in der Domäne ist und die Suchpostfächer nicht mehr benötigt werden müssen folgende Befehle ausgeführt werden um eine reibungslose deinstallation durchzuführen.

```
Get-Mailbox -Arbitration | Disable-Mailbox -Arbitration
```

Durch den ersten Befehl wurden die nur die Mailboxen gelöscht aber nicht die zugehörigen Accounts, im Anschluss kommt folgender Befehl zum Einsatz um die AD Accounts zu löschen.

```
Get-Mailbox -Arbitration | Remove-Mailbox -Arbitration -RemoveLastArbitrationMailboxAllowed
```

#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://therealshrimp.blogspot.de/2010/03/clean-removal-of-first-exchange-2010.html" rel="nofollow">http://therealshrimp.blogspot.de/2010/03/clean-removal-of-first-exchange-2010.html</a>
```