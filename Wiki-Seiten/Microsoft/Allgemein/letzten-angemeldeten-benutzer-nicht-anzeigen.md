---
title: letzten-angemeldeten-benutzer-nicht-anzeigen
description: 
published: true
date: 2023-12-31T14:28:40.328Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:28:37.282Z
---

# Letzten angemeldeten Benutzer nicht anzeigen

Bei der Anmeldemaske soll der letzte Benutzer nicht angezeigt werden, dies kann man über eine Gruppenrichtlinie realisieren oder über eine Regeintrag.

<div class="vector-body" id="bkmrk-gruppenrichtlinie-co"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Gruppenrichtlinie <dl><dd>Computerkonfiguration → Richtlinien → Windows-Einstellungen → Sicherheitseinstellungen → Lokale Richtlinie → Sicherheitsoptionen → "Interaktive Anmeldung: Letzten Benutzernamen nicht anzeigen" auf **1** stellen</dd></dl>
- Regeintrag <dl><dd>HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\Policies\\System → "dontdisplaylastusername" auf **1** stellen.</dd></dl>

</div></div></div>  
Quelle:

```
<a class="external free" href="http://www.maxi-pedia.com/do+not+display+last+user+name" rel="nofollow">http://www.maxi-pedia.com/do+not+display+last+user+name</a>
```