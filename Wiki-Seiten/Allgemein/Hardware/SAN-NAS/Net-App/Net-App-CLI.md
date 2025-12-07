---
title: Net-App CLI
description: Nützliche Befehle um Net-App Speicher über die Command Line zu verwalten
published: true
date: 2025-12-07T13:34:49.359Z
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