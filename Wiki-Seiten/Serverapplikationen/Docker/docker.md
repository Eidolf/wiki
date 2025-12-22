---
title: Docker
description: Docker Befehle und Anleitungen
published: true
date: 2025-12-22T18:03:05.562Z
tags: docker, container, docker compose
editor: markdown
dateCreated: 2023-12-31T13:36:33.797Z
---

# Beschreibung

Systemunabhängige Container Software

# Nützliche Befehle

## Auflisten von Docker Container

`docker ps`

## Auflisten von Docker Images

`docker images`

## Verbindung zu einem laufenden Container herstellen

`sudo docker exec -it [Container-ID oder Name] bash`

### Quelle:

https://www.ionos.de/community/server-cloud-infrastructure/docker/docker-schnellstartanleitung-arbeiten-mit-images-und-containern/

## Log Datei von Container abrufen

`docker logs --tail 50 --follow --timestamps Container_Name`

## Dateien zwischen Host und Docker kopieren

Mit folgendem Befehl wird der **db** Ordner aus dem Docker zum **db** Ordnerpfad auf dem Host kopiert.  
`docker cp dbdocker:/data/db /hostfolder/db`

### Quelle:

https://www.baeldung.com/ops/docker-copying-files

# Erweiterte Aufgaben

## Upgrade einer Postgres Datenbank

### Quelle:

https://josepostiga.com/how-to-upgrade-postgresql-version-and-transfer-your-old-data-using-docker/

# Docker auf Windows

## WSL Virtual Disk verkleinern
Bei Windows Subsystem für Linux (WSL) wurde bei mir die Virtuelle Festplatte leider immer größer obwohl ich meine Docker Versuche bereinigt habe.
Scheinbar ist das nicht ungewöhnlich und man muss sich mit diskpart behelfen um die VHDX zu komprimieren.

### Schritte

1. Alle WSL-Instanzen herunterfahren
Öffne ein Administrator-Kommando-Fenster und gib ein:
`wsl --shutdown`

- Überprüfe, ob alles gestoppt ist:
`wsl.exe --list --verbose`

2. DiskPart starten
PowerShell oder CMD öffnen, danach
`diskpart`

3. Virtuelle Festplatte auswählen
Innerhalb von DiskPart:
`select vdisk file="<Pfad zur VHDX-Datei>"`

- Beispiel:
`select vdisk file="C:\Users\user\AppData\Local\Packages\Docker-Image.vhdx"`
Es sollte die Meldung erscheinen, dass die virtuelle Festplatte erfolgreich ausgewählt wurde.


4. Verkleinern
Führe den Befehl aus:
`compact vdisk`

### Quelle:
https://stackoverflow.com/questions/70946140/docker-desktop-wsl-ext4-vhdx-too-large
