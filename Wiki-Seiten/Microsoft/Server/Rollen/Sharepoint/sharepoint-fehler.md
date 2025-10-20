---
title: sharepoint-fehler
description: 
published: true
date: 2023-12-31T14:35:44.825Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:35:41.793Z
---

# Sharepoint Fehler

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-fehler-401-%28kein-zug-1">Fehler 401 (kein Zugriff)</span>

Quelle:

```
<a class="external free" href="http://www.pentalogic.net/sharepoint-products/reminder/reminder-manual?p=troubleshooting%2F401errors%2F401errors.htm" rel="nofollow">http://www.pentalogic.net/sharepoint-products/reminder/reminder-manual?p=troubleshooting%2F401errors%2F401errors.htm</a>
```

## <span class="mw-headline" id="bkmrk-nur-lokal-am-sharepo-1">Nur lokal am Sharepoint Server kein Zugriff</span>

Am lokalen Server 2000/2003 kann die Sharepoint Seite nicht angezeigt werden, es wird dreimal nach einem Benutzer gefragt und endet in der 401 Fehlermeldung. Von anderen Clients funktioniert die Seite einwandfrei.

  
Schnellste und einfachste möglichkeit (Von Microsoft nicht vorgeschlagen, siehe Quellink):

<div class="vector-body" id="bkmrk-set-the-disablestric"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Set the DisableStrictNameChecking registry entry to 1. For more information about how to do this, click the following article number to view the article in the Microsoft Knowledge Base: 281308 ([http://support.microsoft.com/kb/281308/](http://support.microsoft.com/kb/281308/) ) Connecting to SMB share on a Windows 2000-based computer or a Windows Server 2003-based computer may not work with an alias name
2. Regedit öffnen
3. Im Registrierungseditor folgenden Pfad suchen ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa
    ```
4. Rechts Klick auf **Lsa** &gt; Neu &gt; DWORD
5. DWORD Wert= **DisableLoopbackCheck** und mit ENTER bestätigen.
6. **DisableLoopbackCheck** ändern.
7. Wert= **1** und mit OK bestätigen.
8. Registrierungseditor schließen und den Computer neu starten.

</div></div></div>  
Quelle:

```
<a class="external free" href="http://support.microsoft.com/default.aspx?scid=kb;en-us;896861" rel="nofollow">http://support.microsoft.com/default.aspx?scid=kb;en-us;896861</a>
```