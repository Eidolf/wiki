---
title: Net-App CLI
description: Nützliche Befehle um Net-App Speicher über die Command Line zu verwalten
published: true
date: 2025-11-27T20:35:00.358Z
tags: net-app, speicher, storage, cli
editor: markdown
dateCreated: 2025-09-15T08:14:47.149Z
---

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
`system node halt -node * -ignore-quorum-warnings true - inhibit-takeover true`
2. An Komsole sieht man das sie heunterfahren, Anzeige "Loader-A"
3. Kippschalter umstellen (optional)