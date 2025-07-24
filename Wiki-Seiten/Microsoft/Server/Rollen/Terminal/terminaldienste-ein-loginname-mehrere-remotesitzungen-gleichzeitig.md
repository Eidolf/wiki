---
title: Terminaldienste ein Loginname mehrere Remotesitzungen gleichzeitig
description: 
published: true
date: 2025-07-24T15:02:34.520Z
tags: rdp
editor: markdown
dateCreated: 2023-12-31T14:37:03.183Z
---

Bei einem Benutzer der gleichzeit mehrer RDP Sitzungen auf einen Server ausführen will, muss entweder die Lokale Gruppenrichtlinie geändert werden oder bei einem Terminalserver unter der Terminalserverkonfiguration eine Option geändert werden.

  
Lokale Gruppenrichtlinie ( Local Group Policy Editor ):

`
Computer Configuration\Administrative Templates\Windows Components\Terminal Services\Terminal Server\Connections
`

Terminal Dienst Konfiguration ( Terminal Services Configuration ):

`
General section > Edit Settings > Restict each user to a single session
`

## Quelle:

http://support.microsoft.com/kb/947723/en-us