---
title: scom-alte-ereignisse-konnen-nicht-geschlossen-werden
description: 
published: true
date: 2023-12-31T13:35:13.916Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:35:10.947Z
---

# SCOM Alte Ereignisse können nicht geschlossen werden

In der SCOM Konsole werden alte Einträge angezeigt, wenn man auf diese klickt erscheint eine Fehlermeldung "Object not found".  
  
Lösung:  
Den Verknüpfungspfad zur exe ändern auf \[ "C:\\Program Files\\System Center Operations Manager 2007\\Microsoft.MOM.UI.Console.exe" /clearcache \]  
Dadurch wird der lokale Cache bei jedem neustart gelöscht. Wenn die alten Einträge gelöscht sind kann man es wieder auf den Standard zurückstellen.