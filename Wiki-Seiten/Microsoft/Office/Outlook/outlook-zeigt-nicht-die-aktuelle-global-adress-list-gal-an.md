# Outlook zeigt nicht die aktuelle Global Adress List (GAL) an

Folgende Punkte treffen zu:

<div class="vector-body" id="bkmrk-im-outlook-adressbuc"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Im Outlook Adressbuch werden unter der Globalen Adressliste keine neuen Benutzer, oder Änderungen bestehender Benutzer angezeigt.
2. Das Postfachprofil ist im Cached Modus eingebunden.
3. Mit Outlook Web App tritt dieses Problem nicht auf.
4. Im Outlook Adressbuch wird der lokale Pfad unter den Eigenschaften der Globalen Adressliste angezeigt. (Rechtsklick im Adressbuch auf die GAL → Eigenschaften)
5. Wenn unter folgendem Pfad der Adressbuch Ordner gelöscht wird (hier muss beachtet werden, das bei mehreren verbundenen Exchange Konten auch mehrere Ordner vorhanden sind) " **C:\\Users\\Benutzername\\AppData\\Local\\Microsoft\\Outlook\\Offline Address Books** " und danach Outlook neu gestartet wird, erscheint beim Abruf das aktuelle Adressbuch.

</div></div></div>  
Wenn die meisten Punkte zutreffen ist das Offlineadressbuch am Server defekt und der Client kann sich nicht die aktuelle Version laden. Es kann natürlich auch noch sein das der Pfad zum Offlineadressbuch vom Client aus nicht stimmt, dies kann man folgend nachprüfen.

<div class="vector-body" id="bkmrk-im-infobereich-mit-s"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Im Infobereich mit Strg + Mausklick auf das Outlook Symbol
2. Auf " E-Mail Autokonfiguration testen " klicken
3. Kennwort des Profils angeben
4. Haken bei " Guessmart verwenden " und bei " Sichere Guessmart-Authentifizierung " entfernen
5. Auf " Test " klicken

</div></div></div>Im Ergebnisreiter sollte unter " URL für Offlineadressbuch " der gleiche Kryptische Ordner am Ende der Adresszeile angegeben sein wie auf dem Exchange Server unter **C:\\Program Files\\Microsoft\\Exchange Server\\V14\\ClientAccess\\OAB**  
In beiden fällen muss das Offlineadressbuch neu verknüpft werden. Entweder man versucht jeden Fehler einzeln auszubessern, oder legt das Standard Offline Adressbuch neu an.  
  
siehe → [Exchange Offlineadressbuch](https://wiki.eidolf.de/index.php/Exchange_Offlineadressbuch "Exchange Offlineadressbuch")