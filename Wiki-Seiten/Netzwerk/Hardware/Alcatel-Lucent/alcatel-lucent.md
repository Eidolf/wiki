# Alcatel-Lucent

# <span class="mw-headline" id="bkmrk-konsolen-kabel-1">Konsolen Kabel</span>

Alcatel verwendet eine eigene Belegung für ihr RJ45 zu DB9 Kabel, folgend die Belegung  
[![Alcatel-6xxx-Console.png](https://wiki.eidolf.de/images/1/17/Alcatel-6xxx-Console.png)](https://wiki.eidolf.de/index.php/Datei:Alcatel-6xxx-Console.png)

# <span class="mw-headline" id="bkmrk-authentifizierung-1">Authentifizierung</span>

## <span class="mw-headline" id="bkmrk-standard-anmeldedate-1">Standard Anmeldedaten</span>

Name: admin  
Passwort: switch

## <span class="mw-headline" id="bkmrk-http-freischalten-1">HTTP freischalten</span>

`aaa authentication http local`

# <span class="mw-headline" id="bkmrk-ip-interface-verwalt-1">IP Interface verwalten</span>

## <span class="mw-headline" id="bkmrk-anzeigen-1">Anzeigen</span>

`show ip interface`

## <span class="mw-headline" id="bkmrk-erstellen-1">Erstellen</span>

`ip interface "Interface Name" vlan 1 address 192.168.0.254 mask 255.255.255.0`

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6schen-1">Löschen</span>

`no ip interface "Interface Name"`

# <span class="mw-headline" id="bkmrk-port-mirroring-verwa-1">Port Mirroring verwalten</span>

## <span class="mw-headline" id="bkmrk-anzeigen-3">Anzeigen</span>

`show port mirroring status`

## <span class="mw-headline" id="bkmrk-erstellen-3">Erstellen</span>

`port mirroring 1 source 1/2 destination 1/5`

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6schen-3">Löschen</span>

`no port mirroring 1`

# <span class="mw-headline" id="bkmrk-konfiguration-speich-1">Konfiguration Speicher</span>

## <span class="mw-headline" id="bkmrk-anzeigen-welcher-mod-1">Anzeigen welcher Modus benutzt wird</span>

Es gibt Certified (Schreibgeschützter Modus) und Working  
`show running-directory`

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-speicherm%C3%B6glichkeite-1">Speichermöglichkeiten</span>

### <span id="bkmrk--3"></span><span class="mw-headline" id="bkmrk-running-%3E-working-1">Running &gt; Working</span>

`write memory`

### <span id="bkmrk--4"></span><span class="mw-headline" id="bkmrk-working-%3E-certified-1">Working &gt; Certified</span>

`copy working certified [flash-synchro]`  
**flash-synchro** synchronisiert die Konfiguration auf alle Steckplätze

### <span id="bkmrk--5"></span><span class="mw-headline" id="bkmrk-running-%3E-working%2C-i-1">Running &gt; Working, im Certified Mode</span>

`configuration snapshot all <file>`  
Die Datei muss in **working/boot.cfg** verschoben werden.

## <span class="mw-headline" id="bkmrk-konfiguration-ansehe-1">Konfiguration ansehen</span>

`show configuration snapshot [all|vlan|ip|...]`

## <span class="mw-headline" id="bkmrk-von-certified-in-wor-1">Von Certified in Working Mode wechseln</span>

`reload working no rollback-timeout`

# <span id="bkmrk--6"></span><span class="mw-headline" id="bkmrk-switch-zur%C3%BCcksetzen-1">Switch zurücksetzen</span>

Quelle:

```
<a class="external free" href="http://www.alcatelunleashed.com/viewtopic.php?t=23244" rel="nofollow">http://www.alcatelunleashed.com/viewtopic.php?t=23244</a>
```