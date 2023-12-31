# Google Drive mit Rclone einbinden

# <span class="mw-headline" id="bkmrk-inhalt%3A-1">Inhalt:</span>

Es soll ein google Drive für alle Anwendungen im Netzwerk zur Verfügung stehen. Mit allen nativen Google Anwendungen ist es leider nicht möglich z.B. eine Freigabe für das Netzwerk zu erstellen oder es als Speicher für Backupanwendungen zu verwenden.

# <span class="mw-headline" id="bkmrk-voraussetzungen-1">Voraussetzungen</span>

<div class="vector-body" id="bkmrk-rclone-wird-f%C3%BCr-den-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. rclone <dl><dd>Wird für den Zugriff auf die Clouddienste benötigt</dd><dd>[https://rclone.org/downloads/](https://rclone.org/downloads/)</dd></dl>
2. winfsp <dl><dd>Wird nur auf Windows benötigt um einfach den Ordner als Laufwerk zu mounten</dd><dd>[http://www.secfs.net/winfsp/download/](http://www.secfs.net/winfsp/download/)</dd></dl>
3. nssm <dl><dd>Wird benötigt um das gemountete Laufwerk als System Dienst allen Konten am Server zur Verfügung zu stellen.</dd><dd>[https://nssm.cc/download](https://nssm.cc/download)</dd></dl>

</div></div></div># <span class="mw-headline" id="bkmrk-installation-1">Installation</span>

<div class="vector-body" id="bkmrk-google-api-key-erste"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Google API Key erstellen <dl><dd>Es wird im späteren Verlauf eine Google Drive API Zugriff benötigt, deshalb kann man das gleich als erstes erstellen.</dd><dd>Da ich den Key vor der Anleitung erstellt habe werde ich nur auf einen gut beschriebenen Artikel verlinken.</dd><dd>[https://github.com/Cloudbox/Cloudbox/wiki/Google-Drive-API-Client-ID-and-Client-Secret](https://github.com/Cloudbox/Cloudbox/wiki/Google-Drive-API-Client-ID-and-Client-Secret)</dd></dl>
2. Den entpackten RClone Ordner auf einen Fileserver unter **C:\\Program Files** kopieren
3. winfsp installieren <dl><dd>[![Rclone-001.png](https://wiki.eidolf.de/images/7/7d/Rclone-001.png)](https://wiki.eidolf.de/index.php/Datei:Rclone-001.png)</dd><dd>[![Rclone-002.png](https://wiki.eidolf.de/images/5/55/Rclone-002.png)](https://wiki.eidolf.de/index.php/Datei:Rclone-002.png)</dd><dd>[![Rclone-003.png](https://wiki.eidolf.de/images/6/65/Rclone-003.png)](https://wiki.eidolf.de/index.php/Datei:Rclone-003.png)</dd></dl>
4. **Verstärkte Sicherheitskonfiguration für IE** im Server-Manager für Administratoren deaktivieren.
5. RClone konfigurieren. 
    1. Administrative CMD öffnen
    2. In den RClone Ordner wechseln z.B. **C:\\Program Files\\rclone-v1.48.0-windows-amd64**
    3. Config für eine normale google drive Verbindung erstellen. 
        1. `rclone.exe config`
        2. New Remote `n`
        3. Name `google`
        4. Google Drive `12` (Bitte Nr. überprüfen!)
        5. Client ID und Secret aus Punkt 1 eingeben
        6. Full Access... `1`
        7. Id of the root folder leer lassen und bestätigen
        8. Service Account Credentials JSON file path leer lassen und bestätigen
        9. Edit advanced config? `n` (No)
        10. Use auto config? `y` (Yes)
        11. Benutzername und Passwort des Google Accounts eingeben und der Anwendung rclone vertrauen.
        12. Configure this as a team drive? `n` (No)
        13. Konfiguration überprüfen `y` (Yes this is OK)
        14. Quit config `q`
    4. Config für eine verschlüsselte google drive Verbindung erstellen 
        1. New Remote `n`
        2. Name `secure`
        3. Encrypt/Decrypt a remote `9` (Bitte Nr. überprüfen!)
        4. Zielverbindung eingeben getrennt durch einen : `google:secure`
        5. Encrypt the filenames see the docs for the details. `2` (Alle Dateien sollen verschlüsselt werden)
        6. Don't encrypt directory names, leave them intact. `2` (Bei diesem Einstellungspunkt kann man entscheiden ob auch die Ordnernamen verschlüsselt werden, ich möchte das bei mir nicht haben)
        7. Password or pass phrase for encryption. `y` (Eigenes Passwort verwenden)
        8. Passwort eingeben
        9. Password or pass phrase for salt. `y` (Eigenes Passwort verwenden)
        10. Passwort eingeben
        11. Edit advanced config? `n`
        12. Konfiguration überprüfen `y` (Yes this is OK)
        13. Quit config `q`
6. NSSM konfigurieren 
    1. Die 64Bit Version der nssm.exe in den Rclone Installations Ordner kopieren
    2. Die **rclone.conf** Datei aus dem Ordner **C:\\Users\\Benutzername\\.config\\rclone** in den Rclone Installations Ordner kopieren
    3. In der noch offenen CMD folgenden Befehl eingeben <dl><dd>`nssm.exe install rclone`</dd></dl>
    4. In dem nun geöffneten Fenster folgende Werte ausfüllen. 
        1. Application | Path = Pfad zur rclone.exe
        2. Application | Arguments = `mount secure: g: --config "C:\Program Files\rclone-v1.48.0-windows-amd64\rclone.conf"`
        3. Application | Servicename = `rclone`
        4. Details | Display Name = `Rclone Mount`
        5. Details | Description = `Automatically mounts the encrypted google drive using rclone`
        6. Exit actions | Delay restart by = `10000`ms
    5. Auf **Install Service** klicken
    6. services.msc öffnen
    7. rclone mount Service starten

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://rclone.org/commands/rclone_mount/" rel="nofollow">https://rclone.org/commands/rclone_mount/</a>
<a class="external free" href="https://forum.rclone.org/t/installing-rclone-mount-on-windows-as-service/4649/7" rel="nofollow">https://forum.rclone.org/t/installing-rclone-mount-on-windows-as-service/4649/7</a>
```