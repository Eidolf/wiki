---
title: Net-App CLI
description: Nützliche Befehle um Net-App Speicher über die Command Line zu verwalten
published: true
date: 2025-09-15T08:14:50.173Z
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