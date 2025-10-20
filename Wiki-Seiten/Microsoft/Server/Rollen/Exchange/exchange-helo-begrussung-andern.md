---
title: exchange-helo-begrussung-andern
description: 
published: true
date: 2023-12-31T13:33:16.215Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:33:13.123Z
---

# Exchange HELO Begrüßung ändern

Um die Serverantwort zu ändern muss in der Exchange Shell folgender Befehl eingegeben werden.

```
Set-ReceiveConnector –identity “ServerConnectorname” –Banner “220 mail.domainame.com”
```

Der **Serverconnectorname** ist meist **"Default Frontend ExchangeNetBiosName"**  
Die Zahl **"220"** ist RFC konform.