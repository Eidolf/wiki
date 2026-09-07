---
title: Docker
description: Docker Befehle und Anleitungen
published: true
date: 2026-09-07T17:20:12.182Z
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

# Netzwerk
Standardmäßig sind Docker Netzwerke sehr groß.
Ab **172.16.X.X** Bereich mit **/16** Netzmaske und somit effektiv maximal 16 Docker.

Wenn der 172er Bereich aufgefüllt ist wird als nächstes folgender Bereich verwendet
**192.168.X.X** mit **/20** Netzmaske, hierdurch wird dieser Bereich auch schnell aufgefüllt und ist ebenfalls bei 15 bis 16 Dockern schluss.

Um also mehr als 30 bis 32 Docker auf einem Host zu haben kann man das Standardnetzwerk abändern und eine Vorgabe in der Docker Config geben.

Folgende Config Datei `daemon.json` wird unter dem
Pfad: `/etc/docker/`
angelegt.

Somit in einer Bash
```bash
sudo nano /etc/docker/daemon.json
```
und folgendes hier einfügen und speichern.
```json
{
  "default-address-pools": [
    {
      "base": "172.18.0.0/16",
      "size": 24
    },
    {
      "base": "172.19.0.0/16",
      "size": 24
    },
    {
      "base": "192.168.0.0/16",
      "size": 24
    }
  ]
}
```

Danach den Docker Dienst durchstarten

```bash
sudo systemctl restart docker
```

Die Netze sind hiernach deutlich kleiner und man kann mehr Docker starten.

> Falls das Netz schon voll ist, muss am besten ein Docker, welcher eines der gesetzten Netze blockiert, vor dem Docker Neustart beendet werden.
{.is-info}


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
