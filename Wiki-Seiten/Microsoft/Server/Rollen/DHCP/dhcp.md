---
title: dhcp
description: 
published: true
date: 2023-12-31T13:32:17.855Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:32:13.534Z
---

# DHCP

# <span class="mw-headline" id="bkmrk-dhcp-options-1">DHCP Options</span>

## <span class="mw-headline" id="bkmrk-dhcp-option-119-1">DHCP Option 119</span>

Diese Option wird für eine Auflistung DNS Domain Namen verwendet die meist bei SIP Server Verbindungen nötig sind  
Wird erstellt über **Set Predefined Options**

<div class="vector-body" id="bkmrk-rechtsklick-auf%C2%A0ipv4"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Rechtsklick auf **IPv4**
2. Klick auf **Set Predefined Options**
3. **DHCP Standard Options** lassen und auf **Add** klicken
4. Erstellen mit folgenden Optionen 
    - Name = DNS Search List
    - Data type = String
    - Code = 119
    - Description = DNS Search List
    
    <dl><dt>[![Option119.png](https://wiki.eidolf.de/images/thumb/2/22/Option119.png/300px-Option119.png)](https://wiki.eidolf.de/index.php/Datei:Option119.png)</dt></dl>

</div></div></div>## <span class="mw-headline" id="bkmrk-dhcp-option-120-1">DHCP Option 120</span>

Diese Option wird für einen SIP Server verwendet (z.B. Lync oder Skype for Business)  
Wird erstellt über **Set Predefined Options**

<div class="vector-body" id="bkmrk-rechtsklick-auf%C2%A0ipv4-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Rechtsklick auf **IPv4**
2. Klick auf **Set Predefined Options**
3. **DHCP Standard Options** lassen und auf **Add** klicken
4. Erstellen mit folgenden Optionen 
    - Name = UCSipServer
    - Data type = Binary
    - Code = 120
    - Description = UC Sip Server
    
    <dl><dt>[![Option120.png](https://wiki.eidolf.de/images/thumb/b/b4/Option120.png/300px-Option120.png)](https://wiki.eidolf.de/index.php/Datei:Option120.png)</dt></dl>

</div></div></div>## <span class="mw-headline" id="bkmrk-dhcp-option-252-1">DHCP Option 252</span>

Die Option wird verwendet um über den DHCP eine PAC / WPAD Datei zu verteilen. Somit bekommt jeder Client in dem Scope einen Proxy Server mitgeteilt.  
Grob werden folgende Einstellungen unter den **Predefined Options** gesetzt:

<div class="vector-body" id="bkmrk-name-%3D-wpad-options-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Name = WPAD Options
- Data type = String
- Code = 252

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.how-to-compute.de/2017/02/23/dhcp-wpad-option-252-hinzufuegen/" rel="nofollow">http://www.how-to-compute.de/2017/02/23/dhcp-wpad-option-252-hinzufuegen/</a>
```

## <span class="mw-headline" id="bkmrk-dhcp-option-msucclie-1">DHCP Option MSUCClient</span>

Diese Optionen werden für Lync/S4B Hardware Telefone verwendet  
Die Option muss erst als **Define Vendor Class** angelegt werden

<div class="vector-body" id="bkmrk-rechtsklick-auf%C2%A0ipv4-2"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Rechtsklick auf **IPv4**
2. Klick auf **Define Vendor Classes**
3. Klick auf **Add**
4. Folgende Optionen einstellen 
    - Display Name = MSUCClient
    - Description = UC Vendor Class Id
    - Im Binary Bereich = 4D532D55432D436C69656E74 welches im ASCII Bereich **MS-UC-Client** ergibt
    
    <dl><dt>[![MSUCClient.png](https://wiki.eidolf.de/images/thumb/5/50/MSUCClient.png/300px-MSUCClient.png)](https://wiki.eidolf.de/index.php/Datei:MSUCClient.png)</dt></dl>

</div></div></div>Folgend die zugehörigen **Predefined Options**

<div class="vector-body" id="bkmrk-rechtsklick-auf%C2%A0ipv4-3"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Rechtsklick auf **IPv4**
2. Klick auf **Set Predefined Options**
3. Auf **MSUCClient** umstellen und auf **Add** klicken
4. Erstellen mit folgenden Optionen 
    1. UCIdentifier 
        - Name = UCIdentifier
        - Data type = Binary
        - Code = 1
        - Description = UC Identifier
        
        <dl><dt>[![MSUCClient-Option1.png](https://wiki.eidolf.de/images/thumb/3/35/MSUCClient-Option1.png/300px-MSUCClient-Option1.png)](https://wiki.eidolf.de/index.php/Datei:MSUCClient-Option1.png)</dt></dl>
    2. URLScheme 
        - Name = URLScheme
        - Data type = Binary
        - Code = 2
        - Description = URL Scheme
        
        <dl><dt>[![MSUCClient-Option2.png](https://wiki.eidolf.de/images/thumb/c/c1/MSUCClient-Option2.png/300px-MSUCClient-Option2.png)](https://wiki.eidolf.de/index.php/Datei:MSUCClient-Option2.png)</dt></dl>
    3. WebServerFqdn 
        - Name = WebServerFqdn
        - Data type = Binary
        - Code = 3
        - Description = Web Server FQDN
        
        <dl><dt>[![MSUCClient-Option3.png](https://wiki.eidolf.de/images/thumb/d/d0/MSUCClient-Option3.png/300px-MSUCClient-Option3.png)](https://wiki.eidolf.de/index.php/Datei:MSUCClient-Option3.png)</dt></dl>
    4. WebServerPort 
        - Name = WebServerPort
        - Data type = Binary
        - Code = 4
        - Description = Web Server Port
        
        <dl><dt>[![MSUCClient-Option4.png](https://wiki.eidolf.de/images/thumb/e/e7/MSUCClient-Option4.png/300px-MSUCClient-Option4.png)](https://wiki.eidolf.de/index.php/Datei:MSUCClient-Option4.png)</dt></dl>
    5. CertProvRelPath 
        - Name = CertProvRelPath
        - Data type = Binary
        - Code = 5
        - Description = Certificate Server Relative Path
        
        <dl><dt>[![MSUCClient-Option5.png](https://wiki.eidolf.de/images/thumb/0/00/MSUCClient-Option5.png/300px-MSUCClient-Option5.png)](https://wiki.eidolf.de/index.php/Datei:MSUCClient-Option5.png)</dt></dl>

</div></div></div>## <span class="mw-headline" id="bkmrk-dhcp-options-auslese-1">DHCP Options auslesen</span>

Auf der Quellen Seite gibt es ein PowerShell Script welches einwandfrei die aktuellen Optionen auflistet die der Client aktuell bekommen hat.

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://ingmarverheij.com/read-dhcp-options-received-by-the-client/" rel="nofollow">https://ingmarverheij.com/read-dhcp-options-received-by-the-client/</a>
```

# <span class="mw-headline" id="bkmrk-dhcp-migration-1">DHCP Migration</span>

[DHCP Migration](https://wiki.eidolf.de/index.php/DHCP_Migration "DHCP Migration")

# <span class="mw-headline" id="bkmrk-dhcp-relay-1">DHCP Relay</span>

[DHCP Relay](https://wiki.eidolf.de/index.php/DHCP_Relay "DHCP Relay")