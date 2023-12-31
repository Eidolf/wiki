---
title: skype-for-business-2019
description: 
published: true
date: 2023-12-31T12:04:22.356Z
tags: 
editor: markdown
dateCreated: 2023-12-31T11:55:35.748Z
---

# Skype for Business 2019

## <span class="mw-headline" id="bkmrk-migration-1">Migration</span>

[Migration zu Skype for Business 2019](https://wiki.eidolf.de/index.php/Migration_zu_Skype_for_Business_2019 "Migration zu Skype for Business 2019")

## <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

### <span class="mw-headline" id="bkmrk-skype-adressbuch-kan-1">Skype Adressbuch kann nicht durchsucht werden</span>

#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>
https://ucsorted.com/2017/07/31/skype-federation-search-stops-working-event-id-62044/

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-festplatte-l%C3%A4uft-vol-1">Festplatte läuft voll</span>

Wieder einmal hat Microsoft alles verschlimmbessert. Es gibt scheinbar seit dem S4B 2019 kein Windows Fabric mehr sondern jetzt Microsoft Azure Service Fabric.  
Leider wird dadurch ein Problem nicht behoben und das ist eine vollaufende Festplatte mit Performance Logs die sich nicht überschreiben.  
Wie ich nachgelesen habe gibt es auch keine Möglichkeit es über einen Schalter zu aktivieren und man muss deshalb die Logs einfach manuell oder über ein Script löschen.  
  
Die Pfade in denen die Logs ungehemmt wachsen sind folgende

- C:\\ProgramData\\Microsoft\\SF\\Log\\PerformanceCountersBinaryArchive
- C:\\ProgramData\\Microsofth\\SF\\Log\\PerformanceCounters\_WindowsFabricPerfCounter

Ein Script zum automatischen löschen über eine geplante Aufgabe ist im Link zu finden. Hieraus habe ich auch meine Informationen.  
Ein Hinweis noch zum Script. Es ist ausschließlich als Function aufgebaut und man muss deshalb noch den Aufruf der Function selbst darunter schreiben.

#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>
https://www.ucmadscientist.com/fabric-circular-logging-for-skype-for-business-2019/