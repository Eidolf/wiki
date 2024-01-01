---
title: google-drive-mit-rclone-einbinden
description: 
published: true
date: 2024-01-01T17:00:05.145Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:27:53.746Z
---

# Google Drive mit Rclone einbinden

# <span class="mw-headline" id="bkmrk-inhalt%3A-1">Inhalt:</span>

Es soll ein google Drive für alle Anwendungen im Netzwerk zur Verfügung stehen. Mit allen nativen Google Anwendungen ist es leider nicht möglich z.B. eine Freigabe für das Netzwerk zu erstellen oder es als Speicher für Backupanwendungen zu verwenden.

# <span class="mw-headline" id="bkmrk-voraussetzungen-1">Voraussetzungen</span>

<div class="vector-body" id="bkmrk-rclone-wird-f%C3%BCr-den-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. rclone <dl><dd>Wird für den Zugriff auf die Clouddienste benötigt</dd><dd>[https://rclone.org/downloads/](https://rclone.org/downloads/)</dd></dl>
2. winfsp <dl><dd>Wird nur auf Windows benötigt um einfach den Ordner als Laufwerk zu mounten</dd><dd>[http://www.secfs.net/winfsp/download/](http://www.secfs.net/winfsp/download/)</dd></dl>
3. nssm <dl><dd>Wird benötigt um das gemountete Laufwerk als System Dienst allen Konten am Server zur Verfügung zu stellen.</dd><dd>[https://nssm.cc/download](https://nssm.cc/download)</dd></dl>

</div></div></div># <span class="mw-headline" id="bkmrk-installation-1">Installation</span>

1. Google API Key erstellen <dl><dd>Es wird im späteren Verlauf eine Google Drive API Zugriff benötigt, deshalb kann man das gleich als erstes erstellen.</dd><dd>Da ich den Key vor der Anleitung erstellt habe werde ich nur auf einen gut beschriebenen Artikel verlinken.</dd><dd>[https://github.com/Cloudbox/Cloudbox/wiki/Google-Drive-API-Client-ID-and-Client-Secret](https://github.com/Cloudbox/Cloudbox/wiki/Google-Drive-API-Client-ID-and-Client-Secret)</dd></dl>
2. Den entpackten RClone Ordner auf einen Fileserver unter **C:\\Program Files** kopieren
3. winfsp installieren
![Rclone-001.png](/media/Rclone-001.png)
![Rclone-002.png](/media/Rclone-002.png)
![Rclone-003.png](/media/Rclone-003.png)
4. **Verstärkte Sicherheitskonfiguration für IE** im Server-Manager für Administratoren deaktivieren.
5. RClone konfigurieren. 
    5.1. Administrative CMD öffnen
    5.2. In den RClone Ordner wechseln z.B. **C:\\Program Files\\rclone-v1.48.0-windows-amd64**
    5.3. Config für eine normale google drive Verbindung erstellen. 
        5.3.1. `rclone.exe config`
        5.3.2. New Remote `n`
        5.3.3. Name `google`
        5.3.4. Google Drive `12` (Bitte Nr. überprüfen!)
        5.3.5. Client ID und Secret aus Punkt 1 eingeben
        5.3.6. Full Access... `1`
        5.3.7. Id of the root folder leer lassen und bestätigen
        5.3.8. Service Account Credentials JSON file path leer lassen und bestätigen
        5.3.9. Edit advanced config? `n` (No)
        5.3.10. Use auto config? `y` (Yes)
        5.3.11. Benutzername und Passwort des Google Accounts eingeben und der Anwendung rclone vertrauen.
        5.3.12. Configure this as a team drive? `n` (No)
        5.3.13. Konfiguration überprüfen `y` (Yes this is OK)
        5.3.14. Quit config `q`
    5.4. Config für eine verschlüsselte google drive Verbindung erstellen 
        5.4.1. New Remote `n`
        5.4.2. Name `secure`
        5.4.3. Encrypt/Decrypt a remote `9` (Bitte Nr. überprüfen!)
        5.4.4. Zielverbindung eingeben getrennt durch einen : `google:secure`
        5.5.5. Encrypt the filenames see the docs for the details. `2` (Alle Dateien sollen verschlüsselt werden)
        5.5.6. Don't encrypt directory names, leave them intact. `2` (Bei diesem Einstellungspunkt kann man entscheiden ob auch die Ordnernamen verschlüsselt werden, ich möchte das bei mir nicht haben)
        5.5.7. Password or pass phrase for encryption. `y` (Eigenes Passwort verwenden)
        5.5.8. Passwort eingeben
        5.5.9. Password or pass phrase for salt. `y` (Eigenes Passwort verwenden)
        5.5.10. Passwort eingeben
        5.5.11. Edit advanced config? `n`
        5.5.12. Konfiguration überprüfen `y` (Yes this is OK)
        5.5.13. Quit config `q`
6. NSSM konfigurieren 
    6.1. Die 64Bit Version der nssm.exe in den Rclone Installations Ordner kopieren
    6.2. Die **rclone.conf** Datei aus dem Ordner **C:\\Users\\Benutzername\\.config\\rclone** in den Rclone Installations Ordner kopieren
    6.3. In der noch offenen CMD folgenden Befehl eingeben
    `nssm.exe install rclone`
    6.4. In dem nun geöffneten Fenster folgende Werte ausfüllen. 
        6.4.1. Application | Path = Pfad zur rclone.exe
        6.4.2. Application | Arguments = `mount secure: g: --config "C:\Program Files\rclone-v1.48.0-windows-amd64\rclone.conf"`
        6.4.3. Application | Servicename = `rclone`
        6.4.4. Details | Display Name = `Rclone Mount`
        6.4.5. Details | Description = `Automatically mounts the encrypted google drive using rclone`
        6.4.6. Exit actions | Delay restart by = `10000`ms
    6.5. Auf **Install Service** klicken
    6.6. services.msc öffnen
    6.7. rclone mount Service starten

## Quelle:
https://rclone.org/commands/rclone_mount/
https://forum.rclone.org/t/installing-rclone-mount-on-windows-as-service/4649/7
https://forum.rclone.org/t/installing-rclone-mount-on-windows-as-service/4649/7