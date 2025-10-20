---
title: exchange-postfachgrosse-ansehen
description: 
published: true
date: 2023-12-31T14:33:56.438Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:53.414Z
---

# Exchange Postfachgröße ansehen

Befehl eingeben

```
Get-MailboxStatistics -server "servername" | sort $_.TotalItemSize | FT DisplayName,TotalItemSize
```