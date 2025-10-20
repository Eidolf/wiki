---
title: anwendung-als-dienst-starten
description: 
published: true
date: 2023-12-31T13:28:19.623Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:28:16.639Z
---

# Anwendung als Dienst starten

Wenn ein Programm als Dienst gestartet werden soll kann man mit "sc.exe" arbeiten

<div class="vector-body" id="bkmrk-sc-create-%E2%80%9Etestdiens"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- sc create „Testdienst“ start= auto binpath= „C:\\Programme\\Beispieldatei.exe“

</div></div></div>Zum löschen:

<div class="vector-body" id="bkmrk-sc-delete-testdienst"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- sc delete Testdienst

</div></div></div>Einstellungen zu finden unter:

<div class="vector-body" id="bkmrk-hkey_local_machine%5Cs"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\"erstellter Dienst"

</div></div></div>Wenn der Dienst nicht richtig startet kann man **srvany.exe** aus dem Windows Server 2003 Ressource Kit benutzen (Auch bis Server 2008 R2, hierbei kommt nur eine Problemmeldung bei der Installation)

<div class="vector-body" id="bkmrk-sc-create-%E2%80%9Etestdiens-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. sc create „Testdienst“ start= auto binpath= „C:\\Programme\\Windows Resource Kits\\Tools\\srvany.exe“
2. Regedit öffnen
3. HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\"erstellter Dienst"
4. Schlüssel anlegen mit Namen: **Parameters**
5. Neue Zeichenfolge (String) anlegen mit Namen: **Application**
6. Wert des Schlüssel: **Pfad zur gewünschten Anwendung (auch mit Parametern benutzbar)**

</div></div></div>  
Quelle:

```
<a class="external free" href="http://www.serverhowto.de/Applikationen-als-Dienste-einrichten.228.0.html" rel="nofollow">http://www.serverhowto.de/Applikationen-als-Dienste-einrichten.228.0.html</a>
<a class="external free" href="http://www.wewimo.de/aus-einem-programm-einen-dienst-erstellen-windows-server-2003-danke-sc/" rel="nofollow">http://www.wewimo.de/aus-einem-programm-einen-dienst-erstellen-windows-server-2003-danke-sc/</a>
```