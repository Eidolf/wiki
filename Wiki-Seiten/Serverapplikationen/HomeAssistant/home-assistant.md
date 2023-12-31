# Home Assistant

# <span class="mw-headline" id="bkmrk-datenbank-1">Datenbank</span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-recorder-%28verlauf%29-a-1">Recorder (Verlauf) auf andere Datenbank verschieben</span>

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

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://www.home-assistant.io/integrations/recorder/#:~:text=Home%20Assistant%20uses%20SQLAlchemy%2C%20which,does%20not%20require%20any%20configuration" rel="nofollow">https://www.home-assistant.io/integrations/recorder/#:~:text=Home%20Assistant%20uses%20SQLAlchemy%2C%20which,does%20not%20require%20any%20configuration</a>.
<a class="external free" href="https://community.home-assistant.io/t/moving-recorder-to-ms-sql-install-dependencies/141072/17" rel="nofollow">https://community.home-assistant.io/t/moving-recorder-to-ms-sql-install-dependencies/141072/17</a>
```

# <span class="mw-headline" id="bkmrk-konfiguration-automa-1">Konfiguration automatisch zu GitHub hochladen</span>

## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://www.youtube.com/watch?v=hhv-WqGUy_o" rel="nofollow">https://www.youtube.com/watch?v=hhv-WqGUy_o</a>
```

# <span class="mw-headline" id="bkmrk-windows-ha-1">Windows HA</span>

## <span class="mw-headline" id="bkmrk-ha-in-systray-1">HA in Systray</span>

Ein Popup Fenster das sich in der Taskleiste versteckt und mit schweben über das Icon öffnet.

### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://github.com/mrvnklm/homeassistant-desktop" rel="nofollow">https://github.com/mrvnklm/homeassistant-desktop</a>
```

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-ha-mqtt-status-%C3%9Cberm-1">HA MQTT Status Übermittlung</span>

Hiermit können Werte von Windows an Homeassistant übergeben werden.

### <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="https://github.com/sleevezipper/hass-workstation-service" rel="nofollow">https://github.com/sleevezipper/hass-workstation-service</a>
```