---
title: freepbx
description: 
published: true
date: 2023-12-31T13:36:53.373Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:36:49.163Z
---

# FreePBX

# <span class="mw-headline" id="bkmrk-freepbx-standard-bef-1">FreePBX Standard Befehle</span>

## <span class="mw-headline" id="bkmrk-login-1">Login</span>

### <span class="mw-headline" id="bkmrk-linux-einstellungen-1">Linux Einstellungen</span>

Logon root / PW

### <span class="mw-headline" id="bkmrk-asterisk-einstellung-1">Asterisk Einstellungen</span>

Danach kann man sich mit folgenden Befehl auf die Asterisk Einstellungen einwählen `asterisk -r`

## <span class="mw-headline" id="bkmrk-netzwerk-einstellung-1">Netzwerk Einstellungen</span>

Netwerkeinstellungen werden in ifcfg-ethX Dateien geändert

<div class="vector-body" id="bkmrk-anmelden-als-root-cd"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Anmelden als Root
2. `cd /etc/sysconfig/network-scripts`
3. `nano ifcfg-eth0`(es kann natürlich jeder beliebige Editor verwendet werden um auf eth0 etc. Einstellungen zu kommen)
4. Daten abändern wie benötigt und speichern
5. Neustart des Netzwerk Dienstes `service network restart`

</div></div></div># <span class="mw-headline" id="bkmrk-freepbx---einstellun-1">FreePBX - Einstellungen</span>

## <span class="mw-headline" id="bkmrk-zertifikate-1">Zertifikate</span>

### <span class="mw-headline" id="bkmrk-von-enterprise-ca-au-1">Von Enterprise CA ausstellen</span>

<div class="vector-body" id="bkmrk-admin-%3E-certificate-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Admin &gt; Certificate Management
2. Generate CSR
3. Alles ausfüllen und beim CN den FQDN der FreePBX einfügen &gt; Generate CSR
4. Download des CSR
5. Bei Windows CA als Request einreichen und anschließend als Base64 herunterladen
6. Weiterhin werden die SubCA und RootCA Zertifikate als Base64 benötigt
7. Alle Zertifikate mit einem Texteditor öffnen
8. Weiter in der FreePBX &gt; New Certificate &gt; Upload Certificate
9. Folgendes ausfüllen 
    1. Base DN, den gleichen Namen wie davor (nicht zwingend)
    2. Discription (Beschreibung) ist erforderlich
    3. Passphrase wird leer gelassen da wir den CSR an der FreePBX ausgestellt haben
    4. CSR Reference, den zuvor verwendeten CSR auswählen
    5. Certificate, den Base64 Text des Zertifikats einfügen
    6. Trusted Chain, als erstes den Base64 Text der Sub CA und danach den Base64 Text der Root CA einfügen.
    7. Generate Certificate drücken
    8. Als Default setzen

</div></div></div>#### <span class="mw-headline" id="bkmrk-optional%3A-1">Optional:</span>

Dem Webserver das Zertifikat zuordnen

<div class="vector-body" id="bkmrk-admin-%3E-system-admin"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Admin &gt; System Admin
2. HTTPS Setup
3. Settings
4. Hochgeladenes Zertifikat auswählen
5. Install

</div></div></div># <span class="mw-headline" id="bkmrk-freepbx---skype-for--1">FreePBX - Skype for Business Trunk</span>

Ich habe für meine Installation der PBX zwei Netzwerkkarten gewählt, dies ist nicht notwendig aber meine Firewall wollte nicht so wie ich will. Deswegen ist eine Netzwerkkarte im Perimeternetz und die zweite Richtung intern.

## <span class="mw-headline" id="bkmrk-einstellungen-freepb-1">Einstellungen FreePBX Seite</span>

### <span class="mw-headline" id="bkmrk-settings-1">Settings</span>

#### <span class="mw-headline" id="bkmrk-asterisk-sip-setting-1">Asterisk SIP Settings</span>

<div class="vector-body" id="bkmrk-nat-%3D-no-dynamic-hos"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- NAT = No
- Dynamic Host = Dyn Adresse
- Bind Port = 5061
- Enable TCP = YES
- Other SIP Settings: insecure = port,invite

</div></div></div>### <span class="mw-headline" id="bkmrk-connectivity-1">Connectivity</span>

#### <span class="mw-headline" id="bkmrk-trunks-1">Trunks</span>

<div class="vector-body" id="bkmrk-fritzbox%3A-trunkname-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. FritzBox: 
    - TrunkName FritzBox
    - Outbound CallerID = z.B. 0891234 ohne Durchwahlen
    
    
    1. Outgoing:
    
    - Trunk Name = FritzBox
    - PEER Details =
1. General:
2. sip Settings:


</div></div></div>```
host=IP Adresse Fritzbox
username=620
secret=Passwort für FritzBox
type=peer
qualify=yes
dtmfmode=rfc2833
fromdomain=fritz.box
fromuser=620
disallow=all
allow=alaw&ulaw&g726
insecure=port,invite
```

<div class="vector-body" id="bkmrk-incoming%3A-user-conte"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Incoming:
- User Context = 620
- User Details =




</div></div></div>```
context=from-trunk
secret=Passwort für FritzBox
type=user
insecure=port,invite
fromdomain=fritz.box
disallow=all
allow=alaw&ulaw&g726
```

<div class="vector-body" id="bkmrk-register-string-%3D-62"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Register String = 620:Passwort für FritzBox@IPFritzBox:5060/+49891234XX mit Durchwahl




</div></div></div>Die Nummer hinter dem **/** wird an Skype for Business übergeben und ist in meiner Umgebung von Nöten da ich nur mit einer Telefonnummer arbeiten kann, meine Durchwahlen sind alle ausgedacht und nicht beim Provider bestellt.

<div class="vector-body" id="bkmrk-skype4business%3A-trun"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Skype4Business: 
    - TrunkName Skype4Business
    
    
    1. Outgoing:
    
    - Trunk Name = Skype4Business
    - PEER Details =
1. General:
2. sip Settings:


</div></div></div>```
host=IP Adresse S4B
transport=tcp,udp
port=5060
insecure=port,invite
type=friend
context=from-internal
promiscredir=yes
qualify=yes
canreinvite=yes
```

### <span class="mw-headline" id="bkmrk-connectivity-3">Connectivity</span>

#### <span class="mw-headline" id="bkmrk-outbound-routes-1">Outbound Routes</span>

<div class="vector-body" id="bkmrk-fritzbox%3A-route-name"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. FritzBox: 
    - Route Name=FritzBox
    - Trunk Sequence for matched routes=FritzBox
1. Route Settings:
2. Dial Patterns:


<table class="wikitable"><tbody><tr><th>prepend</th><th>prefix</th><th>match pattern</th><th>Caller ID</th></tr><tr><td> </td><td> </td><td>XZXX.</td><td> </td></tr><tr><td>0</td><td>+49</td><td>XZXX.</td><td> </td></tr></tbody></table>

1. Skype4Business: 
    - Route Name=Skype4Business
    - Trunk Sequence for matched routes=Skype4Business
1. Route Settings:
2. Dial Patterns:


<table class="wikitable"><tbody><tr><th>prepend</th><th>prefix</th><th>match pattern</th><th>Caller ID</th></tr><tr><td> </td><td> </td><td>+49891234XX</td><td> </td></tr><tr><td>+49</td><td>0</td><td>891234XX</td><td> </td></tr></tbody></table>

</div></div></div>#### <span class="mw-headline" id="bkmrk-inbound-routes-1">Inbound Routes</span>

<div class="vector-body" id="bkmrk-all%3A-set-destination"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. All:
- Set Destination = Trunks | Skype4Business


</div></div></div>### <span class="mw-headline" id="bkmrk-admin%3A-1">Admin:</span>

#### <span class="mw-headline" id="bkmrk-config-edit%3A-1">Config Edit:</span>

<div class="vector-body" id="bkmrk-sip_custom.conf"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- sip\_custom.conf

</div></div></div>```
tcpenabled=yes
transport=tcp
```

### <span class="mw-headline" id="bkmrk-skype-for-business-1">Skype for Business</span>

Ab hier kann man jetzt in Skype for Business die weitere Einrichtung vornehmen, beginnend mit dem Mediation Server  
[S4B Mediation Server Einrichten](https://wiki.eidolf.de/index.php/S4B_Mediation_Server_Einrichten "S4B Mediation Server Einrichten")