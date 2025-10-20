---
title: exchange-getrenntes-postfach-loschen
description: 
published: true
date: 2023-12-31T14:33:42.570Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:39.476Z
---

# Exchange getrenntes Postfach löschen

<div class="vector-body" id="bkmrk-guid-der-mailbox-her"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. GUID der Mailbox herausfinden ```
    get-mailboxstatistics -database "Mailbox Database Name" | Where{ $_.DisconnectDate -ne $null } | fl dis*,leg*,Ident*
    ```
2. Erneute Abfrage mit GUID aus Schritt 1 ```
    $test=get-mailboxstatistics -database "Mailbox Database 123456778" | Where{ $_.DisconnectDate -ne $null -and ($_.Identity -like 'GUID') }
    ```
3. Löschen der Mailbox ```
    Remove-mailbox -database $test.database -storemailboxidentity $test.mailboxguidQuelle:
    ```

</div></div></div>Oder mit einem Befehl wenn man GUID des Benutzers hat

```
Remove-mailbox -database "Mailbox Database Name" -storemailboxidentity 'GUID'
```

  
  
Quelle

```
<a class="external free" href="http://www.roland-ehle.de/archives/939" rel="nofollow">http://www.roland-ehle.de/archives/939</a>
```