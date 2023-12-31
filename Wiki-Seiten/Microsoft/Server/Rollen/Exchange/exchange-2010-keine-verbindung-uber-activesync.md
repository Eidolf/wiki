# Exchange 2010 keine Verbindung über ActiveSync

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-fehler%3A-ger%C3%A4t-wird-g-1">Fehler: Gerät wird geblockt</span>

Bei einer ActiveSync verbindung wird Fehlercode " 0x86000C1C " angezeigt.  
Es wird eine Mail an die Betroffene Person gesendet mit folgendem Inhalt.

```
Der Zugriff Ihres Mobiltelefons auf den Server über Exchange ActiveSync wurde aufgrund von Serverrichtlinien verweigert.

Ihr Telefon kann aufgrund einer Zugriffsrichtlinie, die auf dem Server definiert ist,
nicht über Exchange ActiveSync mit dem Server synchronisiert werden.

Gerätemodell: PocketPC 
Gerätetyp: PocketPC 
Geräte-ID: BAD73E6E02156460E800185977C03182 
Gerätebetriebssystem:  
Gerätebenutzer-Agent:  
Geräte-IMEI:  
Exchange ActiveSync-Version: 14.0 
Gerätezugriffsstatus: Blocked 
Grund für Gerätezugriffsstatus: Global 
```

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

<div class="vector-body" id="bkmrk-exchange-shell-%C3%B6ffne"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Exchange Shell öffnen
2. Befehl " Get-ActiveSyncOrganizationSettings " eingeben
3. Überprüfen ob " DefaultAccessLevel " auf **Block** steht
4. Befehl " Set-ActiveSyncOrganizationSettings –DefaultAccessLevel Allow" eingeben
5. Eventuell noch Mal mit " Get-ActiveSyncOrganizationSettings " überprüfen ob nun das " DefaultAccessLevel " auf **Allow** steht.

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
 <a class="external free" href="http://mobilitydojo.net/2009/09/28/restricting-exchange-activesync-access/" rel="nofollow">http://mobilitydojo.net/2009/09/28/restricting-exchange-activesync-access/</a>
```