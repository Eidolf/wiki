---
title: sharepoint-maximalen-upload-vergrossern
description: 
published: true
date: 2023-12-31T14:35:49.471Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:35:46.386Z
---

# SharePoint Maximalen Upload vergrößern

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-server-2008-und-shar-1">Server 2008 und Sharepoint 3.0 Problem bei Uploads über 28 MB</span>

Wenn man versucht Dateien die größer sind als 28 MB auf den Sharepoint zu laden wird eine 404 Fehlerseite angezeigt mit "Seite nicht vorhanden", das gleiche tritt auf wenn man es über die Ordnerstruktur versucht (Fehler: Datei nicht vorhanden)  
Um den Fehler zu beheben muss man unter " **\\inetpub\\wwwroot\\wss\\VirtualDirectories\\** " die " **web.config** " Datei mit einem Editor öffnen und untenstehenden Code unter dem " **&lt;configurations&gt;** " Reiter einfügen (am besten ganz unten in der Datei, da sonst ein Serverfehler auftritt)

```
<system.webServer>
    <security>
        <requestFiltering>
            <requestLimits maxAllowedContentLength="52428800"/>
        </requestFiltering>
    </security>
</system.webServer>
```

Anmerkung: Mit dem eingestellten Wert kann man 50 MB Dateien hochladen.  
  
  
Quelle:

```
<a class="external free" href="http://support.microsoft.com/kb/925083/EN-US/" rel="nofollow">http://support.microsoft.com/kb/925083/EN-US/</a>
```