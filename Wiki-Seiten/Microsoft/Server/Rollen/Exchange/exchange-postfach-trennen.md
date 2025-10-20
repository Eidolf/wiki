---
title: exchange-postfach-trennen
description: 
published: true
date: 2023-12-31T14:33:51.871Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:48.757Z
---

# Exchange Postfach trennen

Das Postfach muss erst deaktiviert werden, erscheint aber nicht sofort unter den getrennten Postfächern. Also entweder warten oder folgenden Befehl ausführen.

```
Clean-MailboxDatabase MyMailboxDatabase*

*MyMailboxDatabase: Muss durch den eigenen Postfachspeicher ersetzt werden
```

  
  
Quelle:

```
<a class="external free" href="http://blog.port389.de/?p=913" rel="nofollow">http://blog.port389.de/?p=913</a>
```