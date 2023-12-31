# SQL Protokoll Logging

# <span class="mw-headline" id="bkmrk-einrichtung">Einrichtung</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SQL_Protokoll_Logging&action=edit&section=1 "Abschnitt bearbeiten: Einrichtung")<span class="mw-editsection-bracket">\]</span></span>

Um eine ein TMG Protokoll direkt auf einen SQL Server zu Speichern müssen mehrere Berechtigungen richtig gesetzt werden. Auch wird für die verschlüsselte Übertragung ein Server Zertifikat benötigt.

Einrichtungsanleitung:

```
<a class="external free" href="http://www.it-training-grote.de/download/TMG-SQL-Logging.pdf" rel="nofollow">http://www.it-training-grote.de/download/TMG-SQL-Logging.pdf</a>
```

## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SQL_Protokoll_Logging&action=edit&section=2 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
TMG: <a class="external free" href="http://technet.microsoft.com/en-us/library/bb838859.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/bb838859.aspx</a>
SQL: <a class="external free" href="http://technet.microsoft.com/en-us/library/bb794867.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/bb794867.aspx</a>
```

# <span class="mw-headline" id="bkmrk-fehler">Fehler</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SQL_Protokoll_Logging&action=edit&section=3 "Abschnitt bearbeiten: Fehler")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-sp1-probleme">SP1 Probleme</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SQL_Protokoll_Logging&action=edit&section=4 "Abschnitt bearbeiten: SP1 Probleme")<span class="mw-editsection-bracket">\]</span></span>

```
siehe <a href="https://wiki.eidolf.de/index.php/TMG_Service_Pack_1#SQL_Logging" title="TMG Service Pack 1">TMG SP1 SQL Logging Problem</a>
```

## <span class="mw-headline" id="bkmrk-sql-logging-bricht-w-1">SQL Logging bricht wegen veralterten Zertifikat ab</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SQL_Protokoll_Logging&action=edit&section=5 "Abschnitt bearbeiten: SQL Logging bricht wegen veralterten Zertifikat ab")<span class="mw-editsection-bracket">\]</span></span>

Fehlermeldung:

<div class="vector-body" id="bkmrk-test-connection-fail"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Test Connection failed because of an error in initializing provider DBNETLIB Connection open SecDoClient Handshake SSL Sicherheitsfehler

</div></div></div>Fehlerbehebung:

<div class="vector-body" id="bkmrk-erneuern-des-sql-zer"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Erneuern des SQL Zertifikats auf dem SQL Server unter &gt; SQL Server Configuration Manager &gt; SQL Server XXX-Netzwerkkonfiguration &gt; Rechtsklick auf "Protokolle für 'MSSQLSERVER'" &gt; Zertifikat

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SQL_Protokoll_Logging&action=edit&section=6 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/ms189067.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/ms189067.aspx</a>
```