---
title: dhcp-relay
description: 
published: true
date: 2023-12-31T13:32:12.084Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:32:08.749Z
---

# DHCP Relay

# <span class="mw-headline" id="bkmrk-aufbau-1">Aufbau</span>

Um ein DHCP Relay aufzubauen werden drei Grundsätzlich Dinge benötigt.

<div class="vector-body" id="bkmrk-ein-zweiter-dhcp-ber"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Ein zweiter DHCP Bereich oder zweiter DHCP Server
- Zwei unterschiedliche Netze
- Routing und RAS aktiviert

</div></div></div>**So konfigurieren Sie den IPv4-DHCP-Relay-Agent**

<div class="vector-body" id="bkmrk-%C3%96ffnen-des-rras-mmc-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Öffnen des RRAS-MMC-Snap-Ins.
2. Erweitern Sie im Routing und RAS-MMC-Snap-In die Option IPv4, und klicken Sie dann auf DHCP-Relay-Agent.
3. Fügen Sie die Netzwerkschnittstellen hinzu, über die auf dem Server möglicherweise DHCP-Anforderungen empfangen werden, die Sie an den DHCP-Server senden möchten. Klicken Sie mit der rechten Maustaste auf DHCP-Relay-Agent, klicken Sie auf Neue Schnittstelle, wählen Sie die entsprechende Netzwerkschnittstelle aus, und klicken Sie dann auf OK.
4. Wählen Sie im Dialogfeld DHCP-Relay die Option DHCP-Pakete weiterleiten aus, und klicken Sie dann auf OK.
5. Klicken Sie im Navigationsbereich mit der rechten Maustaste auf DHCP-Relay-Agent, und klicken Sie dann auf Eigenschaften.
6. Geben Sie auf der Registerkarte Allgemein die IPv4-Adresse des DHCP-Servers ein, für den Sie DHCP-Dienste für die Clients des RRAS-Servers bereitstellen möchten, klicken Sie auf Hinzufügen, und klicken Sie dann auf OK.

</div></div></div>Quelle:

```
<a class="external free" href="http://technet.microsoft.com/de-de/library/dd469685.aspx" rel="nofollow">http://technet.microsoft.com/de-de/library/dd469685.aspx</a>
```

# <span class="mw-headline" id="bkmrk-firewall-1">Firewall</span>

Quelle:

```
<a class="external free" href="http://www.isaserver.org/tutorials/2004dhcprelay.html" rel="nofollow">http://www.isaserver.org/tutorials/2004dhcprelay.html</a>
```