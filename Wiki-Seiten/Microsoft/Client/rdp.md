---
title: RDP
description: Diese Seite befasst sich mit dem Microsoft Remote Desktop Protokoll
published: true
date: 2025-07-24T16:06:41.604Z
tags: microsoft, rdp
editor: markdown
dateCreated: 2025-07-24T16:06:41.604Z
---

# Fehler
## Zwischenablage ( Clipboard ) funktioniert nicht
Es wird keine Richtlinie genutzt um die Zwischenablage in die Remotesitzung zu unterbinden,
weiterhin ist der Haken für die Zwischenablage in der Applikation aktiviert.
![rdp-001.png](/media/rdp-001.png)

Auch wurde in der Registry geprüft das Clipboard grundsätzlich zugelassen ist.
> [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]
> "fDisableClip"=dword:00000000
{.is-info}


In diesem Fall kann es mit einem Fehler bei der `rdpclip.exe` geben.
Auch wenn die **EXE** grundsätzlich ausgeführt wird (siehe Task-Manager), kann es trotzdem zu Problemen kommen.
Der Fehler wäre grundsätzlich mit einem Neustart behoben worden, doch falls das nicht möglich ist, wie ich, einfach den Prozess beenden
und danach wieder starten.