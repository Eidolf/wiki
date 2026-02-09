---
title: Docker Linux
description: Alles rund um Docker und Docker Compose auf Linux
published: true
date: 2026-02-09T14:02:46.379Z
tags: linux, docker, container, docker compose
editor: markdown
dateCreated: 2025-11-01T15:30:26.921Z
---

# Installation
Bevor ich etwas zur Installation schreibe muss ich meinem Unverständnis über die Linux Welt Luft machen.
Hier wird bewusst nichts einfach gestaltet oder sich an Standard Repositories gehalten.
Seit es WinGet auf Windows gibt, wird der Frust immer noch größer wenn ich sehe das man erst sich Repositorys zusammensuchen muss um Software zu installieren, in diesem Fall sowas großes wie Docker.
Aber jetzt zum Thema, ich gestalte es wieder einfach für Windows Nutzer.
Befehl nach Befehl, keine große Erklärung welchen RFC man wieder beachten muss um überhaupt was eingeben zu dürfen.

## Docker und Docker Compose
Hier wieder eine super einfach Version die scheinbar ausreichend ist um beides installiert zu bekommen.
Folgendes hat bei Debian ausgereicht und ich hoffe für Ubuntu klappt das ebenfalls, wenn nicht siehe die nächsten Punkte.
`sudo apt-get install docker-compose`

## Docker
1. `sudo apt-get update`
2. `sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release`
3. `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
4. `echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null`
5. `sudo apt-get update`
6. `sudo apt-get install docker-ce docker-ce-cli containerd.io`
7. `systemctl start docker`
8. `systemctl enable docker`
### Quelle:
https://shape.host/resources/how-to-install-portainer-on-ubuntu-24-04

## Docker Compose
Auch hier musste ich erst wieder ewig umständliche Befehle anschauen bis mir die KI einfach folgendes geschrieben hat.
1. `sudo apt-get update`
2. `sudo apt-get install docker-compose-plugin`
3. `docker compose version`
