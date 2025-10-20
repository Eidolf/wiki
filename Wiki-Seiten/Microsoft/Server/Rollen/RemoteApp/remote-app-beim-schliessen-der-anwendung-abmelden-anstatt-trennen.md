---
title: remote-app-beim-schliessen-der-anwendung-abmelden-anstatt-trennen
description: 
published: true
date: 2023-12-31T14:35:25.828Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:35:22.687Z
---

# Remote App beim schließen der Anwendung abmelden anstatt trennen

Standardmäßig wird beim schließen einer RemoteApp die Verbindung nach ca. 15 - 20sec getrennt und nicht abgemeldet. Das kann man mit folgendem (lokalem) Gruppenrichtlinieneintrag ändern.

```
Computer Computerkonfiguration\Administrative Vorlagen\Windows Components\Remote Desktop Services\
Remote Desktop Session Host\Session Time Limits\Zeitlimit für die Abmeldung bei RemoteApp-Sitzungen festlegen
```

Die hier eingegebene Zeit wird auf die 15-20sec addiert und eine Abmeldung erfolgt

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://support.microsoft.com/kb/2287493/en-us" rel="nofollow">http://support.microsoft.com/kb/2287493/en-us</a>
```