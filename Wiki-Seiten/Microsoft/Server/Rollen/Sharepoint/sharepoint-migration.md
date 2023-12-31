# Sharepoint Migration

**Ausgangslage:**

<div class="vector-body" id="bkmrk-quellserver-spsalt.f"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Quellserver spsalt.firma.local (Server 2008 Standard Edition X64 mit SharepointServices 3.0 SP1 X64)
2. Zielserver spsneu.firma.local (Server 2008 Standard Edition X64)

</div></div></div>**Vorbereitung:**

<div class="vector-body" id="bkmrk-installieren-von-sha"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Installieren von SharepointServices 3.0 SP1 X64 auf dem Zielserver (nur Basisinstallation gemäß Assistent)
2. Deaktivieren der Benutzerkontensteuerung auf dem Quell- und Zielserver (kann Fehlermeldung „Zugriff verweigert“ verursachen bei der Sicherung)

</div></div></div>**Sicherung und Wiederherstellung:**

<div class="vector-body" id="bkmrk-eingabeaufforderung-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Eingabeaufforderung auf dem Quellserver öffnen
2. Wechseln in das Verzeichnis ‘‘C:\\Program Files\\Common Files\\Microsoft Shared\\Web Server Extensions\\12\\BIN“
3. Sicherung mit dem folgendem Befehl starten: stsadm.exe -o backup -url [http://spsalt](http://spsalt/) -filename c:\\sicherung-spsalt\\backup.dat -overwrite <dl><dd>(Jetzt wird die Sicherung in dem angegebenen Pfad angelegt)</dd></dl>
4. Sicherung auf den Zielserver kopieren
5. Eingabeaufforderung auf dem Zielserver öffnen
6. Auf dem Zielserver in das Verzeichnis ‘‘C:\\Program Files\\Common Files\\Microsoft Shared\\Web Server Extensions\\12\\BIN“ wechseln
7. Wiederherstellung mit folgendem Befehl starten: stsadm.exe -o restore -url [http://spsneu](http://spsneu/) -filename c:\\sicherung-spsalt\\backup.dat -overwrite
8. Jetzt die SharePoint 3.0-Zentraladministration öffnen
9. Unter „Anwendungsverwaltung“ - „Richtlinie für Webanwedung“ einen Administrator mit Berechtigungen auf Sharepoint-Seiten-Benutzer hinzufügen
10. Jetzt sollten die Sharepoint Seiten über die neue URL [http://spsneu/default.aspx](http://spsneu/default.aspx) verfügbar sein

</div></div></div>**Nachbereitung:**

<div class="vector-body" id="bkmrk-eventuell-m%C3%BCssen-log"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Eventuell müssen Logo´s und sonstige Bilder manuell nachgepflegt werden

</div></div></div>  
  
Das oben dargestellte Szenario umfasst nur die wirklichen Grundlagen für eine Sicherung und die Wiederherstellung auf einem anderen System.

Quelle:

```
Irgend ein Forum, muss ich suchen
```