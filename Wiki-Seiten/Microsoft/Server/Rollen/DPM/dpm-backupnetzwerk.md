---
title: dpm-backupnetzwerk
description: 
published: true
date: 2023-12-31T14:31:42.088Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:31:38.873Z
---

# DPM Backupnetzwerk

# <span class="mw-headline" id="bkmrk-einrichten%3A-1">Einrichten:</span>

## <span class="mw-headline" id="bkmrk-dns%3A-1">DNS:</span>

### <span class="mw-headline" id="bkmrk-am-server%3A-1">Am Server:</span>

<div class="vector-body" id="bkmrk-neue-zone-am-dns-ser"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Neue Zone am DNS Server einrichten
2. Primäre Zone auswählen und die Zone im AD bereitstellen
3. Forward Zone auswählen
4. Zone benennen wie z.B. anstatt **test.local** für das Backupnetzwerk **testbackup.local**

</div></div></div>### <span class="mw-headline" id="bkmrk-am-client%3A-1">Am Client:</span>

<div class="vector-body" id="bkmrk-tcp%2Fipv4-einstellung"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. TCP/IPv4 Einstellungen des Backupnetzwerks öffnen
2. DNS Server im Backupnetzwerk hinzufügen
3. Unter den erweiterten Einstellungen bei **Registerkarte DNS**, den **testbackup.local Suffix** angeben und den Haken bei **"Diesen Suffix bei der DNS Registrierung benutzen"** setzen

</div></div></div>## <span class="mw-headline" id="bkmrk-dpm-shell%3A-1">DPM Shell:</span>

Folgenden Befehl für das Beispiel Backupnetzwerk 192.168.0.x eingeben

```
Add-BackupNetworkAddress -DpmServername DPM -Address 192.168.0.0/24 -SequenceNumber 1
```

Alle Ausweichbackupnetzwerke werden mit einer höheren Sequenz Nummer angegeben

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/cc964298.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/cc964298.aspx</a>
<a class="external free" href="http://technet.microsoft.com/fr-fr/gg241300.aspx" rel="nofollow">http://technet.microsoft.com/fr-fr/gg241300.aspx</a> (französisch)
```