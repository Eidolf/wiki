# Docker

# <span class="mw-headline" id="bkmrk-beschreibung-1">Beschreibung</span>

Systemunabhängige Container Software

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-n%C3%BCtzliche-befehle-1">Nützliche Befehle</span>

## <span class="mw-headline" id="bkmrk-auflisten-von-docker-1">Auflisten von Docker Container</span>

`docker ps`

## <span class="mw-headline" id="bkmrk-auflisten-von-docker-3">Auflisten von Docker Images</span>

`docker images`

## <span class="mw-headline" id="bkmrk-verbindung-zu-einem--1">Verbindung zu einem laufenden Container herstellen</span>

`sudo docker exec -it [Container-ID oder Name] bash`

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://www.ionos.de/community/server-cloud-infrastructure/docker/docker-schnellstartanleitung-arbeiten-mit-images-und-containern/" rel="nofollow">https://www.ionos.de/community/server-cloud-infrastructure/docker/docker-schnellstartanleitung-arbeiten-mit-images-und-containern/</a>
```

## <span class="mw-headline" id="bkmrk-log-datei-von-contai-1">Log Datei von Container abrufen</span>

`docker logs --tail 50 --follow --timestamps Container_Name`

## <span class="mw-headline" id="bkmrk-dateien-zwischen-hos-1">Dateien zwischen Host und Docker kopieren</span>

Mit folgendem Befehl wird der **db** Ordner aus dem Docker zum **db** Ordnerpfad auf dem Host kopiert.  
`docker cp dbdocker:/data/db /hostfolder/db`

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://www.baeldung.com/ops/docker-copying-files" rel="nofollow">https://www.baeldung.com/ops/docker-copying-files</a>
```

# <span class="mw-headline" id="bkmrk-erweiterte-aufgaben-1">Erweiterte Aufgaben</span>

## <span class="mw-headline" id="bkmrk-upgrade-einer-postgr-1">Upgrade einer Postgres Datenbank</span>

### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://josepostiga.com/how-to-upgrade-postgresql-version-and-transfer-your-old-data-using-docker/" rel="nofollow">https://josepostiga.com/how-to-upgrade-postgresql-version-and-transfer-your-old-data-using-docker/</a>
```