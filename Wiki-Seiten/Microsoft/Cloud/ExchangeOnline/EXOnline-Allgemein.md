---
title: EXOnline-Allgemein
description: Informationen zu Exchange Online
published: true
date: 2024-05-28T15:58:46.858Z
tags: exchange, office365, e-mail
editor: markdown
dateCreated: 2024-05-28T15:58:01.363Z
---

# Exchange Online Allgemein
## Microsoft forciert onPrem Exchange Version
Zur Absicherung forciert Exchange Online gewisse Mindestversionen von onPrem Exchange Servern in Hybrid Umgebungen.
Zum Stand 28.05.2024 sind es alle Versionen >= 15.01.2507.031 (Exchange Server 2016 CU23 Aug23SU).
Folgende Meldung erscheint im Exchange Admin Center unter Reporting
![exchange-block-old-onprem.png](/media/exchange-block-old-onprem.png)

Falls es keine Möglichkeit gibt den Exchange auf den aktuellen Stand zu bekommen kann man im Exchange Admin Center unter Reporting eine Pausierung dieser Funktion aktivieren.
Microsoft lässt pro Jahr nur gesamt 90 Tage an Verzögerung zu.

Einstellungen zur Pausierung ist hier zu finden:
EAC > Reports > Mail flow > Out-of-date connecting on-premises Exchange servers > Enforcement Pause
![exchange-out-of-date-connection_001.png](/media/exchange-out-of-date-connection_001.png)

![exchange-out-of-date-connection_002.png](/media/exchange-out-of-date-connection_002.png)

![exchange-out-of-date-connection_003.png](/media/exchange-out-of-date-connection_003.png)

### Quelle:
https://techcommunity.microsoft.com/t5/exchange-team-blog/how-to-pause-throttling-and-blocking-of-out-of-date-on-premises/ba-p/4007169
https://practical365.com/microsoft-block-old-exchange-servers/