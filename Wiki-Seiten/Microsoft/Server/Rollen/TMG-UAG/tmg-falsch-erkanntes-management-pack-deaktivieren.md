---
title: tmg-falsch-erkanntes-management-pack-deaktivieren
description: 
published: true
date: 2023-12-31T14:36:19.389Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:36:15.198Z
---

# TMG Falsch erkanntes Management Pack deaktivieren

Wenn man z.B. ein neues Management Pack des Forefront TMG installiert werden neue Skripte auch an ISA Server übergeben mit denen dieser aber nicht umgehen kann. Hier sollen nun die ISA Server von dem neuen Management Pack abgekoppelt werden.

<div class="vector-body" id="bkmrk-zum-erkennen-welche-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Zum erkennen welche Rechner an ein Management Pack gebunden sind folgendes durchführen. 
    - Monitoring Reiter --&gt; Discovered Inventory --&gt; Action Pane --&gt; Change target type --&gt; nach z.B. TMG suchen --&gt; weiterhin TMG Server auswählen
    
    <dl><dd>Es werden jetzt alle Server angezeigt die unter dem Management Pack "TMG" gelistet sind.</dd></dl>
2. Da nicht alle Rechner einzeln gelöscht werden sollen muss man eine Gruppe anlegen, eine "ISA" Gruppe ist standarmäßig aber schon vorhanden, deswegen nehmen wir diese. Mit dieser Gruppen werden folgende Einstellungen vorgenommen. 
    - Authoring Reiter --&gt; Object Discoveries --&gt; Nach "TMG" suchen --&gt; TMG Server Discovery markieren
    - Rechtsklick --&gt; Overrides --&gt; Override the Object Discovery --&gt; For a group --&gt; ISA Server Computer auswählen
    - Bei Overrides "Enable" anhaken --&gt; Overide Value auf "False" stellen --&gt; Als destination management pack ein anderes auswählen als das "Default", am besten eines anlegen das für die gesamte Umgebung gilt --&gt; Speichern mit "OK"
3. Zum schluss muss noch die "Operations Manager Command Shell" geöffnet und folgender Befehl abgesetzt werden: ```
     Remove-DisabledMonitoringObject
    ```
4. Da nach diesem Befehl nichts direkt passiert kann man das Ergebnis wie unter Punkt 1 beschrieben nachprüfen.

</div></div></div>  
**Quelle**

```
<a class="external free" href="http://blogs.technet.com/jonathanalmquist/archive/2008/09/14/remove-disabledmonitoringobject.aspx" rel="nofollow">http://blogs.technet.com/jonathanalmquist/archive/2008/09/14/remove-disabledmonitoringobject.aspx</a>
```