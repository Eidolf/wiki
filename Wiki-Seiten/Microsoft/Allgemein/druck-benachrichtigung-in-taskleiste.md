---
title: druck-benachrichtigung-in-taskleiste
description: 
published: true
date: 2023-12-31T14:28:17.118Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:28:14.048Z
---

# Druck Benachrichtigung in Taskleiste

Um die aufploppende Druckbenachrichtigung in der Taskleiste abzuschalten muss man folgenden Regeintrag setzen.

```
HKEY_CURRENT_USER\Printers\Settings "EnableBalloonNotificationsLocal" DWORD:"0"
```