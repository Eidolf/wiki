---
title: home-assistant
description: 
published: true
date: 2026-07-15T14:45:07.763Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:36:38.302Z
---

# Home Assistant

# Datenbank

## Recorder (Verlauf) auf andere Datenbank verschieben

Beim verschieben zu MS-SQL ist folgendes zu beachten.  
Die Verbindung funktioniert nur wenn SQL und Windows Authentifizierung an der SQL Instanz aktiviert wurde, eine pure Windows Authentifizierung funktioniert nicht.  
Es muss ein SQL Benutzer angelegt werden dem man am einfachsten die DBO Berechtigung auf einer neu angelegten Homeassistant Datenbank gibt.  
Falls man HassIO als Betriebssystem nutzt sind von der Homeassistant Seite alle Voraussetzungen erfüllt und man muss nur noch folgende Zeilen in die **configuration.yaml** eintragen.

```
# Database for Records
recorder:
  purge_keep_days: 60
  db_url: mssql+pyodbc://SQLBenutzer:Passwort@HomeassistantIP/Datenbankname?charset=utf8;DRIVER={FreeTDS};Port=1433;
```

### Quelle:

https://www.home-assistant.io/integrations/recorder/#:~:text=Home%20Assistant%20uses%20SQLAlchemy%2C%20which,does%20not%20require%20any%20configuration
https://community.home-assistant.io/t/moving-recorder-to-ms-sql-install-dependencies/141072/17

## Home Assistant InfluxDB Add-on: nofile-Limit temporär erhöhen und .tsm.bad-Dateien retten

Diese Anleitung beschreibt einen temporären Runtime-Fix für das InfluxDB-v1-Add-on unter Home Assistant OS, wenn InfluxDB mit `too many open files` startet und TSM-Dateien als `.tsm.bad` umbenannt wurden.

Der Fix setzt im laufenden Add-on-Container vor dem Start von `influxd` das Open-File-Limit auf `65536`, startet nur den internen s6-Service neu und benennt anschließend die betroffenen `.tsm.bad`-Dateien zurück.

### Voraussetzungen

SSH-Zugriff mit Docker-Rechten, zum Beispiel über das Advanced SSH & Web Terminal Add-on.

Alle Befehle werden auf der SSH-Shell ausgeführt, nicht innerhalb der Home-Assistant-Core-Konfiguration.

### InfluxDB-Container automatisch finden ###

```
APP="$(docker ps --format '{{.Names}}' | grep -i influx | head -n1)"
echo "$APP"
```

Wenn kein Container gefunden wird, alle Container anzeigen:
`docker ps -a --format '{{.Names}}' | grep -i influx`

Falls nötig, den Namen manuell setzen:
`APP="addon_a0d7b954_influxdb"`

### Aktuelles Open-File-Limit von influxd prüfen

`docker exec "$APP" sh -c 'PID=$(pidof influxd); echo "PID=$PID"; cat /proc/$PID/limits | grep "open files"'`

Problematisch ist zum Beispiel:
`Max open files 1024 524288 files`

Ziel ist später:
`Max open files 65536 524288 files`

### Aktive s6-Run-Datei für InfluxDB finden
```
RUNFILE="$(docker exec "$APP" sh -c 'for f in /run/s6/legacy-services/influxdb/run /run/service/influxdb/run /etc/services.d/influxdb/run; do if [ -f "$f" ] && grep -q "exec influxd" "$f"; then echo "$f"; exit 0; fi; done')"

echo "$RUNFILE"
```

In vielen Fällen ist das:
`/run/s6/legacy-services/influxdb/run`

### Aktuelle Run-Datei anzeigen

`docker exec "$APP" sh -c "cat '$RUNFILE'"`

Typischer Inhalt am Ende:
`exec influxd`

### Run-Datei sichern und ulimit-Zeile einfügen
```
docker exec "$APP" sh -c "cp '$RUNFILE' '$RUNFILE.bak' && grep -q 'ulimit -n 65536' '$RUNFILE' || sed -i '/exec influxd/i ulimit -n 65536 || echo \"WARN: could not raise nofile limit\"' '$RUNFILE'"
```

### Patch prüfen

`docker exec "$APP" sh -c "grep -nE 'ulimit|exec influxd' '$RUNFILE'"`

Erwartete Ausgabe:
```
ulimit -n 65536 || echo "WARN: could not raise nofile limit"
exec influxd
```

### Internen s6-Service-Pfad finden
```
SVC="$(docker exec "$APP" sh -c 'for d in /run/service/influxdb /run/s6/legacy-services/influxdb; do if [ -d "$d" ]; then echo "$d"; exit 0; fi; done')"
echo "$SVC"
```

### Nur den InfluxDB-Service neu starten

Nicht das komplette Add-on neu starten, da sonst die Runtime-Datei wieder zurückgesetzt werden kann.
`docker exec "$APP" s6-svc -r "$SVC"`

Einige Sekunden warten und danach prüfen:
`docker exec "$APP" sh -c 'PID=$(pidof influxd); echo "PID=$PID"; cat /proc/$PID/limits | grep "open files"'`

Erwartetes Ergebnis:
`Max open files 65536 524288 files`

### InfluxDB-Datenverzeichnis automatisch finden
```
DATADIR="$(docker exec "$APP" sh -c 'for d in /data/influxdb/data /var/lib/influxdb/data; do if [ -d "$d" ]; then echo "$d"; exit 0; fi; done')"
echo "$DATADIR"
```

### Bereits umbenannte .tsm.bad-Dateien suchen

`docker exec "$APP" sh -c "find '$DATADIR' -name '*.tsm.bad' -print"`


### InfluxDB-Service stoppen, aber Container laufen lassen

`docker exec "$APP" s6-svc -d "$SVC"`

Prüfen, ob influxd gestoppt ist:
`docker exec "$APP" sh -c 'pidof influxd || echo "influxd stopped"'`

### .tsm.bad-Dateien zurückbenennen
`docker exec "$APP" sh -c "find '$DATADIR' -name '*.tsm.bad' -exec sh -c 'for f do mv \"\$f\" \"\${f%.bad}\"; done' sh {} +"`

Danach kontrollieren:
`docker exec "$APP" sh -c "find '$DATADIR' -name '*.tsm.bad' -print"`
Wenn keine Ausgabe erscheint, wurden alle .tsm.bad-Dateien zurückbenannt.

### InfluxDB-Service wieder starten

`docker exec "$APP" s6-svc -u "$SVC"`

Einige Sekunden warten und Limit erneut prüfen:
`docker exec "$APP" sh -c 'PID=$(pidof influxd); echo "PID=$PID"; cat /proc/$PID/limits | grep "open files"'`

### Logs prüfen
`docker logs --tail 100 "$APP"`

Optional fortlaufend beobachten:
`docker logs -f "$APP"`

### Erwartetes Ergebnis
InfluxDB sollte ohne too many open files starten.
Das Open-File-Limit von influxd sollte bei 65536 oder höher liegen.
Die zurückbenannten TSM-Dateien sollten wieder eingelesen werden.
Wichtiger Hinweis zur Dauerhaftigkeit
Dieser Fix ist ein Runtime-Hack im laufenden Add-on-Container.
Bei einem vollständigen Add-on-Neustart, Add-on-Update, Container-Recreate oder Home-Assistant-OS-Neustart kann die Änderung wieder verloren gehen.
In diesem Fall muss der Patch erneut angewendet werden, solange das Add-on das Open-File-Limit nicht selbst dauerhaft beim Start erhöht.

# Konfiguration automatisch zu GitHub hochladen

## Quelle:

https://www.youtube.com/watch?v=hhv-WqGUy_o

# Windows HA

## HA in Systray

Ein Popup Fenster das sich in der Taskleiste versteckt und mit schweben über das Icon öffnet.

### Quelle:

https://github.com/mrvnklm/homeassistant-desktop

## HA MQTT Status Übermittlung

Hiermit können Werte von Windows an Homeassistant übergeben werden.

### Quelle:

https://github.com/sleevezipper/hass-workstation-service