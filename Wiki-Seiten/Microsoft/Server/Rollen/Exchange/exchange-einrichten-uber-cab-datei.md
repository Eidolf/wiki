---
title: exchange-einrichten-uber-cab-datei
description: 
published: true
date: 2023-12-31T14:33:32.319Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:29.102Z
---

# Exchange Einrichten über cab Datei

Notepad öffnen  
Code einfügen

```
<wap-provisioningdoc>
   <characteristic type="Sync">
        <characteristic type="Settings">
        <parm name="SyncWhenRoaming" value="1"/>
        </characteristic>
        <characteristic type="Connection">
            <parm name="Domain" value="company"/>
            <parm name="Password" value="password"/>
            <parm name="SavePassword" value="1"/>
            <parm name="Server" value="mail.company.com"/>
            <parm name="User" value="Joe Bloggs"/>
            <parm name="URI" value="Microsoft-Server-ActiveSync"/>
        <parm name="UseSSL" value="1"/>
        </characteristic>
        <characteristic type="Mail">
            <parm name="Enabled" value="1"/>
        <parm name="EmailAgeFilter" value="3"/>
        </characteristic>
        <characteristic type="Calendar">
            <parm name="Enabled" value="1"/>
        <parm name="CalendarAgeFilter" value="5"/>
        </characteristic>
        <characteristic type="Contacts">
            <parm name="Enabled" value="1"/>
        </characteristic>
        <characteristic type="Tasks"
            <parm name="Enabled value="1"/>
        </characteristic>
   </characteristic>
</wap-provisioningdoc>
```

Werte anpassen:  
Werte die ausgelassen werden können:

```
 <characteristic type="Settings">
       <parm name="SyncWhenRoaming" value="1"/>
```

Bedeutet das nur Synchronisiert wird wenn ein wechsel zwischen Netzübergängen stattfindet.

```
 <parm name="Password" value="password"/>
```

Diese Zeile herauslöschen da es im Klartext steht, Sicherheitsmäßig jeden selbst überlassen.  
  
Werte zum anpassen

```
<parm name="CalendarAgeFilter" value="5"/>
```

Dieser Wert bedeutet 1 Monat, der Wert 3 bedeutet 1 Woche. Diese Werte varieren aber von Gerät zu Gerät, deshalb muss man etwas damit spielen.  
  
Textdatei speichern wo man Sie findet mit dem expleziten Namen \_setup.xml  
CMD öffnen und zum Pfad der Datei wechseln  
Eingabe " MakeCAB.exe /D COMPRESS=OFF \_setup.xml ActiveSync.cab "  
Fertige Datei auf das Smartphone schieben und installieren.

Funktioniert auf WM 5,6 definitiv bis 6.5

Link:

```
<a class="external free" href="http://www.windowsphoneexpert.com/connection/forums/t/342.aspx" rel="nofollow">http://www.windowsphoneexpert.com/connection/forums/t/342.aspx</a>
```