---
title: standarddrucker-kann-nicht-geandert-werden
description: 
published: true
date: 2023-12-31T14:29:07.718Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:29:04.725Z
---

# Standarddrucker kann nicht geändert werden

Unter folgenden Registry Einträgen sollte die Berechtigungsstufe des angemeldeten Benutzers überprüft werde, dieser benötigt auf den Windows Schlüssel nämlich "Vollzugriff"

```
HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\Devices
"%printername%"="winspool,Ne00:"
"%printername%"="winspool,Ne01:"

HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\Windows
"Device"="%printername%,winspool,Ne01:"
```

Ansonsten müssen im Devices Schlüssel alle Drucker eingetragen sein die vorhanden sind also z.B. folgende Zeichenfolge

```
Canon Drucker 1 = winspool,Ne00:
Canon Drucker 2 = winspool,Ne01:
Samsung Drucker 1 = winspool,Ne02:
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://forums.citrix.com/message.jspa?messageID=1432761" rel="nofollow">http://forums.citrix.com/message.jspa?messageID=1432761</a>
```