---
title: outlook-mail-versand-empfang
description: 
published: true
date: 2023-12-31T14:30:13.774Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:30:10.664Z
---

# Outlook Mail Versand / Empfang

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span class="mw-headline" id="bkmrk-automatischer-mailve-1">Automatischer Mailverkehr funktioniert nicht</span>

### <span class="mw-headline" id="bkmrk-firewall-1">Firewall</span>

Mails können hinter einer Firewall nur Empfangen / Versendet werden wenn man explizit auf den Schalter (Senden-Empfangen) drückt.  
Hierzu müssen ausser dem RPC Port 135 noch zusätzliche folgende Ports geöffnet werden.

<div class="vector-body" id="bkmrk-6001-%28information-st"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- 6001 (Information Store)
- 6002 (Directory Referral)
- 6004 (DSProxy/NSPI)

</div></div></div>  
Quelle:  
Abschnitt " Microsoft Exchange Server and Outlook clients "

```
 <a class="external free" href="http://support.microsoft.com/default.aspx?scid=kb;en-us;832017" rel="nofollow">http://support.microsoft.com/default.aspx?scid=kb;en-us;832017</a>
```