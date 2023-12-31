# Teams

Falls man im Jahr &gt;= 2021 auf die Idee kommt seine Benutzer nach Teams zu migrieren kommt man mit der GUI nicht mehr weit.  
Wenn man sich an den aktuellen Patchstand hält und eine Hypbrid Umgebung hat, kam man um die Installation des Teams PowerShell Modul nicht umher.  
Ab dem Zeitpunkt konnte die GUI keine modernen Befehle des Teams PowerShell Moduls nutzen da die Skype PowerShell Module hardcoded verankert wurde.  
Deshalb bleibt nichts anderes übrig als die PowerShell zu verwenden.  
  
Befehl zum migrieren eines Benutzers zu Teams:

<dl id="bkmrk-move-csuser-benutzer"><dt>`Move-CsUser Benutzer@Domain.de -Target sipfed.online.lync.com`  
</dt></dl>In meiner Umgebung musste ich den **-Force** Schalter bei einzelnen Benutzern verwenden, in einer Firmenumgebung würde ich es tunlichst vermeiden da hierdurch Kontakte verloren gehen.

<dl id="bkmrk-move-csuser-benutzer-1"><dt>`Move-CsUser Benutzer@Domain.de -Target sipfed.online.lync.com -Force`</dt></dl>