# Automatischer Admin Login

Um einen automatischen Login zu vollziehen, der nur Sinn macht wenn ein einziger Benutzer (Administrator) an diesem Rechner vorhanden ist muss man in der Registry unter

<div class="vector-body" id="bkmrk-hkey_lokal_machine--"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Hkey\_lokal\_machine -&gt; Software -&gt; Microsoft -&gt; Windows NT -&gt; CurrentVersion -&gt; winlogon

</div></div></div>den Schlüssel **Admin Auto Login** auf **1** stellen.

oder

<div class="vector-body" id="bkmrk-start-%3E-ausf%C3%BChren-%3E%C2%A0"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Start &gt; Ausführen &gt; **control userpasswords2** eingeben

</div></div></div>hiermit öffnet sich die Benuzterverwaltung. In der Benutzerverwaltung den Haken **Benutzer müssen Benutzernamen und Kennwort eingeben** entfernen. Dadurch öffnet das Fenster Automatische Anmeldung, dort kann der entsprechende Benutzername mit Kennwort eingetragen werden der automatisch beim hochfahren angemeldet werden soll.

Quelle:

```
<a class="external free" href="http://www.administrator.de/index.php?content=27545" rel="nofollow">http://www.administrator.de/index.php?content=27545</a>
```