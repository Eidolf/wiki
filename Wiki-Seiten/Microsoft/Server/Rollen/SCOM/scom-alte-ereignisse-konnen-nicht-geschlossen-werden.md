# SCOM Alte Ereignisse können nicht geschlossen werden

In der SCOM Konsole werden alte Einträge angezeigt, wenn man auf diese klickt erscheint eine Fehlermeldung "Object not found".  
  
Lösung:  
Den Verknüpfungspfad zur exe ändern auf \[ "C:\\Program Files\\System Center Operations Manager 2007\\Microsoft.MOM.UI.Console.exe" /clearcache \]  
Dadurch wird der lokale Cache bei jedem neustart gelöscht. Wenn die alten Einträge gelöscht sind kann man es wieder auf den Standard zurückstellen.