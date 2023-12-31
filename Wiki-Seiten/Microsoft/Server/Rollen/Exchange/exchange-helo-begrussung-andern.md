# Exchange HELO Begrüßung ändern

Um die Serverantwort zu ändern muss in der Exchange Shell folgender Befehl eingegeben werden.

```
Set-ReceiveConnector –identity “ServerConnectorname” –Banner “220 mail.domainame.com”
```

Der **Serverconnectorname** ist meist **"Default Frontend ExchangeNetBiosName"**  
Die Zahl **"220"** ist RFC konform.