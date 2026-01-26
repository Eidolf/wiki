---
title: Net-App CLI
description: Nützliche Befehle um Net-App Speicher über die Command Line zu verwalten
published: true
date: 2026-01-26T14:59:36.919Z
tags: net-app, speicher, storage, cli
editor: markdown
dateCreated: 2025-09-15T08:14:47.149Z
---

> Diese gesamte Seite ist mit Vorsicht zu betrachten, ich bein kein NetApp Experte und mache das nur mit "learning by doing". Abfragen sollte kein Problem darstellen und bei allen Konfigurationen bei welchen Datenverlust stattfinden kann, werde ich einen weiterer Warnungsblock im Wiki eingefügen.
{.is-warning}

# Abfragen
## Speicher von VServern
- Aufbau
`vol show -vserver "vserver Name"`
- Beispiel (Name von vserver = freigaben1)
`vol show -vserver freigaben1`

# Aktionen
## Speicher von VServern erweitern
- Aufbau
`vol size -vserver "vserver Name" -volume "Volume Name" +"Speichergröße in MB,GB,TB"`
- Beispiel (Name von vserver = freigaben1, Volume = Datengrab, 10GB mehr Speicher)
`vol size -vserver freigaben1 -volume Datengrab +10GB`

## Herunterfahren
1. Befehl eingeben
`system node halt -node * -ignore-quorum-warnings true -inhibit-takeover true`
2. An Komsole sieht man das sie heunterfahren, Anzeige "Loader-A"
3. Kippschalter umstellen (optional)

# Erweiterte Root Partitions Verteilung
> Folgende Anleitung hat schlussendlich nicht funktioniert und wurde nach dem Cluster Wizard zerstört. Somit lasse ich diese Anleitung nur stehen, um zu sehen was ausprobiert wurde.
{.is-danger}

> Hier wird definitiv Datenverlust stattfinden, ich konnte diese Aufgabe ein paar mal durchtesten, da ich auf grüner Wiese angefangen habe.
{.is-warning}
## Einleitung
Ich habe zwei Shelfs im Einsatz, eines mit 
Shelf 1 (24 3,5 Zoll Schächte)
- 12 x 8TB Platten in den oberen Slots
- 12 x 2TB Platten in den unteren Slots
Shelf 2 (24 2,5 Zoll Schächte)
- 24 x 1,2TB Platten

**Shelf 2** ist kein Problem, da die Platten nicht automatisch für das System Root verwendet werden, aber bei Shelf 1 konnte ich nicht unterteilen und der Automatismus hat immer Root quer über alle Platten verteilt.
In meinen Augen konnte somit nicht der volle Speicherplatz der großen Platten genutzt werden und es musste mit Tricks gearbeitet werden.

## Vorgehen
### Kurze Zusammenfassung
Ich denke dieser ganze "Trick" ist nur mit ADP Initialisierung möglich.

FSAS sind die großen Platten
BSAS sind die kleinen Platten

Root soll ausschließlich auf die kleineren Platten

### Ausgeführte Befehle
1. System ist im Halt Modus
**Anzeige `Loader A/B`**
Hier folgenden Befehl:
`LOADER> boot_ontap`

2. Unter dem Laden der Komponenten wird irgendwann gefragt ob man mit
**Ctrl-C** drücken ins Boot-Menü will, 
Ja möchte man

3. Im Boot Menü 
`(9) Configure Advanced Drive Partitioning`

4. Danach
`(9a) Unpartition all disks and remove their ownership information.`

5. Wenn das durch ist, kann es evtl. verschieden weiter gehen, bei mir folgendermaßen.
`(5)  Maintenance mode boot.`
dort rein um danach einen 

6. `halt` durchzuführen

> Die ganzen bisherigen Befehle habe ich relativ Zeitgleich auf beiden Nodes durchgeführt
{.is-info}

7. Große Disk physisch entfernen

8. Wieder in Boot Menü leider nochmal alles (jedenfalls bei mir)
`9 > 9a > 9b`

9. Nach dem Neustart ist man im Cluster Wizard, hier mit Exit rausgehen.

10. Dann muss man einfach mit admin ohne Passwort in den SP Modus

11. Prüfen ob Root auf die BSAS Platten verteilt wurde mit 
`storage disk show -partition-ownership`

12. Mit `halt` wieder in den Loader Modus

13. FSAS (große Disks) wieder einbauen

14. `boot_ontap`

15. Entweder nochmal prüfen ob die Root Partition nur auf den kleinen Disks liegt (bei mir war es der Fall) oder gleich den Cluster Wizard durchlaufen.


# ONTAP 9.8P21 – CIFS über LAG (ifgrp) einrichten und LIF sauber migrieren
Ziel
Ich möchte eine CIFS‑LIF auf eine LACP‑Bündelung (ifgrp, z. B. a0a) legen.
Weil die ONTAP 9.8‑GUI Broadcast Domains nicht vollständig bearbeiten kann, nutze ich GUI + CLI:

GUI: ifgrp (LAG) anlegen/prüfen, LIF migrieren  
CLI: Ports zur Broadcast Domain hinzufügen (Pflicht, sonst Fehlermeldung „An eligible port was not found“)

## 1. ifgrp (LAG) am Node anlegen/prüfen (GUI)

System Manager öffnen.  
Network → Ethernet Ports (oder Settings → Network je nach 9.8‑Build).  
Interface Groups / LAGs anzeigen.  
Eine neue ifgrp erstellen oder prüfen (z. B. a0a):

Mode: LACP (active)  
Member Ports: meine gewünschten physischen Ports (z. B. e0c, e0d oder 10 GbE‑Ports)  
Load Distribution/Hash: IP oder MAC+IP (für SMB i. d. R. optimal)

Sicherstellen, dass die ifgrp up ist.

Hinweis: Ich lege pro Node eine eigene a0a an (Node‑lokal, LAGs sind nicht node‑übergreifend).

## 2. a0a der richtigen Broadcast Domain zuordnen (CLI)
Die GUI von ONTAP 9.8 kann das nicht zuverlässig – deshalb CLI.

2.1 LIF‑Kontext prüfen (Name der Broadcast Domain herausfinden)

```
network interface show -vserver <SVMNAME> -lif <LIFNAME> -fields ipspace,broadcast-domain,home-node,home-port,address
```

Ich merke mir IpSpace und Broadcast‑Domain der LIF (z. B. Default / Data).

2.2 Prüfen, ob a0a sichtbar ist

```
network port show -node <NODE2> -port a0a
```

2.3 a0a zur Broadcast Domain hinzufügen  
Wichtig: Ich nutze genau den IpSpace und die Broadcast Domain der LIF aus 2.1.

```
network port broadcast-domain add-ports \
  -ipspace <IPSPACE> \
  -broadcast-domain <BDOM> \
  -ports <NODE2>:a0a
```

Beispiel:

```
network port broadcast-domain add-ports \
  -ipspace Default \
  -broadcast-domain Data \
  -ports eidnas02:a0a
```

2.4 Verifizieren

```
network port broadcast-domain show -ipspace <IPSPACE> -broadcast-domain <BDOM>
```

Jetzt sehe ich node2:a0a in der Port‑Liste dieser Domain.

Ohne diesen Schritt bekommt die LIF‑Migration den Fehler „An eligible port was not found“.

## 3. (Optional) Failover‑Group passend setzen (GUI oder CLI)  
Ich möchte, dass meine CIFS‑LIF nur auf die passenden Ports (beider Nodes) failovern darf.

GUI:  
Network → Failover Groups → Gruppe öffnen oder erstellen → a0a von node1 und node2 hinzufügen → Save.

CLI:

```
# Gruppe erstellen (falls nicht vorhanden)
network interface failover-groups create -vserver <SVMNAME> -failover-group cifs_fg

# Ports hinzufügen
network interface failover-groups add-ports \
  -vserver <SVMNAME> -failover-group cifs_fg \
  -ports <NODE1>:a0a,<NODE2>:a0a

# LIF der Failover Group zuweisen
network interface modify -vserver <SVMNAME> -lif <LIFNAME> -failover-group cifs_fg
```

## 4. LIF auf a0a migrieren und Home‑Port setzen (GUI oder CLI)

4.1 Live‑Migration (ohne Downtime für SMB3)

GUI:  
SVM → Network Interfaces → LIF auswählen → Migrate →  
Destination Node auswählen → Destination Port: a0a → Apply

CLI:

```
network interface migrate \
  -vserver <SVMNAME> \
  -lif <LIFNAME> \
  -destination-node <NODE2> \
  -destination-port a0a
```

4.2 Home‑Port anpassen

GUI:  
LIF Edit → Home `Node/Port` auf `<NODE2>/a0a` stellen.

CLI:

```
network interface modify \
  -vserver <SVMNAME> \
  -lif <LIFNAME> \
  -home-node <NODE2> \
  -home-port a0a
```

Tipp:  
Mit

```
network interface show -failover -vserver <SVMNAME> -lif <LIFNAME>
```

sehe ich alle eligible Failover Targets.

## 5. LACP/SMB‑Optimierung (Best Practices)

- LACP Mode: active (Switch‑seitig active/active)
- Hash/Distribution: IP oder MAC+IP
- SMB Multichannel aktiv
- MTU konsistent (1500 oder 9000)
- Mindestens 1–2 CIFS‑LIFs pro Node

## 6. Typische Fehler & schnelle Checks

Fehler: *An eligible port was not found*  
Ursache: a0a ist nicht in der richtigen Broadcast Domain / IpSpace / Failover‑Group.

Checkliste:

```
network interface show -vserver <SVM> -lif <LIF> -fields ipspace,broadcast-domain,home-node,home-port,address
network port show -node <NODE> -port a0a
network port broadcast-domain show -ipspace <IPSPACE> -broadcast-domain <BDOM>
network interface show -failover -vserver <SVM> -lif <LIF>
```

## 7. Rollback

```
network interface migrate -vserver <SVM> -lif <LIF> -destination-node <ALT_NODE> -destination-port <ALT_PORT>
network interface modify -vserver <SVM> -lif <LIF> -home-node <ALT_NODE> -home-port <ALT_PORT>
network port broadcast-domain remove-ports \
  -ipspace <IPSPACE> \
  -broadcast-domain <BDOM> \
  -ports <NODE2>:a0a
```

## 8. Plattform‑Hinweise (FAS2554)

- ifgrp/LACP ist pro Node separat  
- HA erfolgt über LIF‑Failover  
- SMB profitiert von Multichannel & Transparent Failover

Kurzfazit:

- GUI: ifgrp/LAG anlegen, LIF migrieren, Failover Group setzen  
- CLI: a0a der Broadcast Domain hinzufügen  
- Danach keine „eligible port“‑Fehler mehr.

# Volume unterbrechungsfrei in ein anderes Aggregat verschieben (ONTAP Volume Move)
Diese Anleitung gilt für alle ONTAP‑Systeme mit FlexVol Volumes.

## 1. Ausgangslage prüfen
Bevor ein Volume verschoben wird, müssen folgende Daten geprüft werden:
1.1 Welches Volume soll verschoben werden?
`volume show -volume <VOLUME_NAME> -fields aggregate,state,tiering-policy`
1.2 Größe und Nutzung des Volumes prüfen
`volume show -volume <VOLUME_NAME> -fields size,used,available`
1.3 Welches Aggregat ist das neue Ziel?
Kapazität prüfen:
`aggr show -aggregate <ZIEL_AGGR> -fields size,available,state`

## 2. Platz im Zielaggregat sicherstellen
Wenn das Zielaggregat zu wenig Platz hat, müssen alte oder ungenutzte Volumes gelöscht oder verschoben werden.
2.1 Volumes im Zielaggregat anzeigen
`volume show -aggregate <ZIEL_AGGR> -fields volume,size,used`
2.2 Nicht benötigtes Volume löschen
(Nur wenn sicher!)
```
volume offline -vserver <SVM> -volume <VOLUME>
volume delete -vserver <SVM> -volume <VOLUME>
```
2.3 Alternativ: Volume in anderes Aggregat verschieben
`volume move start -vserver <SVM> -volume <VOLUME> -destination-aggregate <ANDERES_AGGR>`

## 3. Unterbrechungsfreien Volume‑Move starten
Wenn genug Platz vorhanden ist, kann das Volume „live“ verschoben werden.
Volume Move starten
`volume move start -vserver <SVM> -volume <VOLUME> -destination-aggregate <ZIEL_AGGR>`

ONTAP führt dabei:
- Block‑Copy im Hintergrund
- anschließenden „Cutover“ mit typischer Unterbrechung < 10 Sekunden durch.

Hyper‑V, VMware, LUN‑basiert, NFS, SMB → alles bleibt online.

## 4. Status des Volume‑Moves überwachen
Status prüfen
`volume move show`

Detaillierter Status
`volume move show-details`

## 5. Abschlussphase: Cutover
ONTAP führt einen kurzen „Cutover“ durch, bei dem das Volume final umgeschwenkt wird.
Falls manuell erforderlich:
`volume move trigger-cutover -vserver <SVM> -volume <VOLUME>`
Dies passiert normalerweise automatisch.

## 6. Nachkontrolle
6.1 Prüfen, ob Volume im neuen Aggregat liegt
`volume show -volume <VOLUME> -fields aggregate`
6.2 LUN‑Mappings prüfen (optional)
LUNs werden automatisch korrekt übernommen:
`lun show -fields path,mapped`
6.3 Eventuelle SnapMirror/SnapVault Beziehungen
`snapmirror show`

Hinweise & Best Practices
- Volume Move ist vollständig online und für Produktions‑VMs geeignet
- Vorher immer Platz im Zielaggregat prüfen
- Aggregate müssen vom selben HA‑Pair sein
- FlexVols bis mehrere hundert TB → problemlos
- Volume Move erzeugt Last → am besten nicht während Backup‑Fenstern