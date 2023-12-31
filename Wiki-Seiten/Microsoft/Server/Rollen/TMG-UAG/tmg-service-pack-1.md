# TMG Service Pack 1

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span class="mw-headline" id="bkmrk-sql-logging-1">SQL Logging</span>

Logdateien die auf einen Unternehmens SQL Server geschrieben werden haben Probleme mit Arytmetischen Sonderzeichen (- + usw.) in Datenbanknamen. Wenn z.B. die Standardinstanz der Datenbank "TMG-Firewall" heißt, versucht er bei einer Abfrage "Firewall" von "TMG" zu subtrahieren, was aber eine falsche Abfrage ist.

  
Nachprüfe kann man es folgendermaßen:

<div class="vector-body" id="bkmrk-sql-profiler-%C3%B6ffnen-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. SQL Profiler öffnen
2. Suche im Log nach dem Dienstekontonamen mit dem der SQL angesprochen wird (z.B. TMGUser)
3. Herausfinden welche Abfragen versucht werden mit dem Benutzer an die Datenbank abzusetzen.
4. Abfrage kopieren und manuell im SQL Manager gegen die Datenbank absetzen.
5. Im Fall einer Fehlermeldung kann man nun überprüfen was die Ursache ist.

</div></div></div>## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-logging-anzeige-von--1">Logging Anzeige von älteren Einträgen</span>

Nach dem Update Rollup 1 für TMG SP1 werden alle alten Einträge in der Protokollabfrage nicht angezeigt. Die Sofortprotokollierung funktioniert aber einwandfrei.  
Lösung:  
Sprache für den NT AUTHORITY\\NetworkService auf Swedish umstellen.  
Vorgehnsweise:

<div class="vector-body" id="bkmrk-anmelden-am-tmg-als-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Anmelden am TMG als lokaler Administrator
- Installation von Microsoft SQL Server 2008 Management Studio Express
- Verbinden mit der entsprechenden Datenbank wie folgt:

</div></div></div>  
Server type: Database Engine  
Server name: Server-Name\\msfw  
Authentication: Windows Authentication

<div class="vector-body" id="bkmrk-punkt-security-erwei"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Punkt Security erweitern
- Punkt Logins erweitern
- NT AUTHORITY\\NetworkService mit rechter Maustaste auswählen und Properties wählen
- unter General - default language auf Swedish umstellen
- SQL Dienste neustarten

</div></div></div>  
  
Quelle:

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/de-DE/forefrontde/thread/03c296d5-9657-4e64-b8c6-c93b3220233f" rel="nofollow">http://social.technet.microsoft.com/Forums/de-DE/forefrontde/thread/03c296d5-9657-4e64-b8c6-c93b3220233f</a>
```