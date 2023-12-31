# Thread Management Gateway

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-forefront-tmg-2010-c-1">Forefront TMG 2010 Computer Zertifikat anfordern oder erneuern schlägt fehl</span>

<div class="vector-body" id="bkmrk-neue-regel-erstellen"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Neue Regel erstellen <dl><dd>TMG (localhost) &gt; CA | Ports: TCP 49152-65535 | Alle Benutzer</dd></dl>
2. Unter den Systemrichtlinien striktes RPC bei Active Directory deaktivieren

</div></div></div>Die ausführliche Beschreibung von Richard Hicks im Quell Link.

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://tmgblog.richardhicks.com/2014/04/21/forefront-tmg-2010-computer-certificate-request-or-renewal-fails/" rel="nofollow">https://tmgblog.richardhicks.com/2014/04/21/forefront-tmg-2010-computer-certificate-request-or-renewal-fails/</a>
```

# <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-multicast-f%C3%BCr-regele-1">Multicast für Regelerstellung aktivieren</span>

<div class="vector-body" id="bkmrk-routing-ras-mmc-%C3%B6ffn"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Routing RAS MMC öffnen
2. Alles bis IPv4 aufklappen und ein Rechtsklick auf Allgemein, hier auf ein **Neues Routingprotokoll** klicken  
    [![TMG-IGMP-001.png](https://wiki.eidolf.de/images/f/fd/TMG-IGMP-001.png)](https://wiki.eidolf.de/index.php/Datei:TMG-IGMP-001.png)
3. IGMP auswählen
4. Auf das neu angelegte IGMP Protokoll einen Rechtsklick und eine **Neue Schnittstelle** auswählen  
    [![TMG-IGMP-002.png](https://wiki.eidolf.de/images/c/c8/TMG-IGMP-002.png)](https://wiki.eidolf.de/index.php/Datei:TMG-IGMP-002.png)
5. Jeweils benötigte Schnittstellen hinzufügen.
6. Am TMG ein neues Protokoll erstellen  
    [![TMG-IGMP-003.png](https://wiki.eidolf.de/images/thumb/d/df/TMG-IGMP-003.png/700px-TMG-IGMP-003.png)](https://wiki.eidolf.de/index.php/Datei:TMG-IGMP-003.png)
7. Folgende Einstellungen (IP Stufe | 2 | Senden Empfangen)  
    [![TMG-IGMP-004.png](https://wiki.eidolf.de/images/0/05/TMG-IGMP-004.png)](https://wiki.eidolf.de/index.php/Datei:TMG-IGMP-004.png)

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://blogs.technet.microsoft.com/isablog/2009/02/04/how-to-enable-multicast-routing-with-isa-server/" rel="nofollow">https://blogs.technet.microsoft.com/isablog/2009/02/04/how-to-enable-multicast-routing-with-isa-server/</a>
```

# <span class="mw-headline" id="bkmrk-tls-1.2-aktivieren-1">TLS 1.2 aktivieren</span>

Die Aktivierung wird am besten mit Nartac Software [IIS Crypto](https://www.nartac.com/Products/IISCrypto) durchgeführt  
Hier einfach Best Practice ausführen

Falls es aber zu Problemen mit "supplied sspi channel bindings were incorrect 0x80090346" bei einzelnen Regeln kommt liegt es scheinbar an folgenden Einträgen die gelöscht werden müssen.  
  
Pfad: `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

```
AllowInsecureRenegoClients
AllowInsecureRenegoServers
UseScsvForTls
```

## <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://gnawgnu.blogspot.de/2011/09/tmg-2010-and-enabling-tls-12.html" rel="nofollow">https://gnawgnu.blogspot.de/2011/09/tmg-2010-and-enabling-tls-12.html</a>
```