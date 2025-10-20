---
title: exchange-2010-probleme-mit-outlook-2003
description: 
published: true
date: 2023-12-31T14:33:03.359Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:32:59.791Z
---

# Exchange 2010 Probleme mit Outlook 2003

# <span class="mw-headline" id="bkmrk-freigegebene-kalende-1">Freigegebene Kalender haben Verbindungsprobleme</span>

<div class="vector-body" id="bkmrk-die-transferverschl%C3%BC"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Die Transferverschlüsselung zwischen dem Outlook 2003 Client und dem Exchange 2010 Server im Outlookprofil anschalten
- Den RCAMaxConcurrency Wert auf 100 setzen und auf alle Mailboxen anwenden die mit Outlook 2003 arbeiten. <dl><dd>Das die Änderung sofort übernommen wird muss man eine AD Replikation anstoßen und den Neustart des "Throttling Service", "RPCClientAccess Service" auf dem CAS und MBX Servers.</dd></dl>

</div></div></div># <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span>

```
<a class="external free" href="http://www.msxfaq.de/clients/outlook2003e2010.htm" rel="nofollow">http://www.msxfaq.de/clients/outlook2003e2010.htm</a>
<a class="external free" href="http://support.microsoft.com/kb/2006508/en-us?fr=1" rel="nofollow">http://support.microsoft.com/kb/2006508/en-us?fr=1</a>
<a class="external free" href="http://blogs.technet.com/b/exchange/archive/2010/04/23/3409853.aspx" rel="nofollow">http://blogs.technet.com/b/exchange/archive/2010/04/23/3409853.aspx</a>
```