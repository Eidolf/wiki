---
title: exchange-2016-hybrid-konfiguration
description: 
published: true
date: 2023-12-31T14:33:08.406Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:05.104Z
---

# Exchange 2016 Hybrid Konfiguration

Der Microsoft Hybridwizard funktioniert eigentlich ganz gut, doch wenn bei nachträglichen Änderungen Probleme auftreten kann es bei der Fehlersuche richtig nervig werden.

# <span class="mw-headline" id="bkmrk-hybrid-wizard-instal-1">Hybrid Wizard Installation</span>

Beim Download kann schon das erste Problem entstehen, deshalb folgendes beachten.

<div class="vector-body" id="bkmrk-den-downloadlink-nur"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Den Downloadlink nur im Internet Explorer öffnen
- Der Download sollte am besten unter dem Benutzer ausgeführt werden der Exchange Administrationsrechte hat.
- Das spätere Ausführen des Hybridwizards nicht Administrativ öffnen, nur als normaler Benutzer.

</div></div></div>Hier ist der Link zum Download [https://aka.ms/HybridWizard](https://aka.ms/HybridWizard)  
Den Link habe ich von der Microsoft Seite in der Quellenbeschreibung.

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/en-us/exchange/hybrid-configuration-wizard-faqs" rel="nofollow">https://docs.microsoft.com/en-us/exchange/hybrid-configuration-wizard-faqs</a>
```

# <span class="mw-headline" id="bkmrk-fehleranalyse-1">Fehleranalyse</span>

Die einzelnen Punkt welche andere Personen schon gut dokumentiert haben nochmal aufzuschreiben wäre viel zu lange. Ich verlinke hier nur meine Quellen die mir bei der Fehlersuche geholfen haben.

<div class="vector-body" id="bkmrk-das-hier-ist-ein-gut"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-das-hier-ist-ein-gut-1" lang="de"><div class="mw-parser-output">- Das hier ist ein guter Einstieg da der Eintrag pro Richtung durchgegangen wird &gt; [https://support.microsoft.com/en-us/help/2555008/how-to-troubleshoot-free-busy-issues-in-a-hybrid-deployment-of-on-prem](https://support.microsoft.com/en-us/help/2555008/how-to-troubleshoot-free-busy-issues-in-a-hybrid-deployment-of-on-prem)
- Dieser Link hat bei meinem Fehler schlussendlich geholfen &gt; [https://techcommunity.microsoft.com/t5/exchange-team-blog/demystifying-hybrid-free-busy-what-are-the-moving-parts/ba-p/607704](https://techcommunity.microsoft.com/t5/exchange-team-blog/demystifying-hybrid-free-busy-what-are-the-moving-parts/ba-p/607704)
- Dieser Link geht auch tiefgreifend in die Fehlersuche und beinhaltet das untenstehende PDF &gt; [https://techcommunity.microsoft.com/t5/exchange-team-blog/demystifying-hybrid-free-busy-finding-errors-and-troubleshooting/ba-p/607727](https://techcommunity.microsoft.com/t5/exchange-team-blog/demystifying-hybrid-free-busy-finding-errors-and-troubleshooting/ba-p/607727)
- Dieses PDF ist sehr Umfangreich &gt; [https://techcommunity.microsoft.com/legacyfs/online/media/2019/01/FB\_Errors.FixesV6.pdf](https://techcommunity.microsoft.com/legacyfs/online/media/2019/01/FB_Errors.FixesV6.pdf)

</div></div></div>