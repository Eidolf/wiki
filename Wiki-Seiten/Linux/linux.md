---
title: linux
description: 
published: true
date: 2024-01-03T09:10:20.802Z
tags: linux, ubuntu, cronjob, webserver, sudo
editor: markdown
dateCreated: 2023-12-31T13:28:07.229Z
---

# Linux

# Allgemein

## Bash

### Überschriebene Bash wiederherstellen

1. Das System mit gedrückter shift Taste starten oder bei einer VM ist es besser eine Linux CD einzulegen und den Recover Mode zu starten
2. Wenn das System mit shift gestartet wurde muss man **e** drücken und folgendes in der Zeile die mit **Linux** startet hinten einfügen `init=/bin/sh`, danach Strg+X zum speichern.
3. Bei beidem sollte man in der Dashshell landen
4. Als nächstes die Standard Bash umändern `chsh username -s /bin/sh`
5. Neustart
6. Anmelden
7. Normale Bash neu installieren`sudo apt-get install --reinstall bash`
8. Die Standard Bash wieder zurückändern auf die normale `sudo chsh username -s /bin/bash`

#### Quelle:
https://askubuntu.com/questions/935528/how-do-i-recover-a-deleted-or-overwritten-bash

chsh username -s /bin/sh reboot your system, now you can login successfully and you'll have a dash shell, reinstall your bash:

sudo apt-get install --reinstall bash then change your default shell to bash:

sudo chsh username -s /bin/bash 

**When you still have a running terminal:**
As a bones, if you ever removed a program which has a running instance you can easily recover it from "procfs", in case of bash if you had a terminal running bash you could fix the bash by running:

sudo cp /proc/$$/exe /bin/bash

## Berechtigung

### Berechtigung auslesen

Für einen Ordner mit den darin liegenden Dateien  
`ls -al /Ordnerpfad`

### Gruppenberechtigung

#### Gruppenberechtigung auslesen

Man gibt den Benutzer an von dem man die Gruppenzugehörigkeit auslesen möchte  
`groups Benutzer`

#### Berechtigung auf Gruppe erteilen

`sudo useradd -G Gruppe Benutzer`

#### Berechtigung auf Gruppe löschen

`sudo gpasswd --delete Benutzer Gruppe`

## Cronjob

Es gibt viele Beispiele, aber als Merker für OSTicket wird ein Cronjob folgendermaßen eingerichtet  
Beispiel sollte alle 5 Minuten mit dem Benutzer nobody das php Script cron.php ausführen.

```
user@server:~$ nano /etc/cron.d/osticket 

*/5 * * * * nobody php /var/www/pfadosticket/api/cron.php
```

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * command to execute
```

### Quelle:
https://en.wikipedia.org/wiki/Cron

## Disk Größe

Wird der Befehl **df** verwendet  
z.B. `df` oder `df /dev/sda1`  
Um die Disk in Gigabyte anzuzeigen folgenden Befehl verwenden  
`df -BG`  
Auch ein guter Schalter ist **-h**, hier wird die Übersicht und Größe automatisch eingestellt.  
`df -h`

## Ordner Größe

Wird der Befehl **du** verwendet  
Um z.B. für einen Ordner die Gesamtgröße herauszufinden  
`du -sh /var/lib/Ordner`

### Quelle:

[http://www.kavoir.com/2009/09/linux-check-how-much-disk-storage-each-directory-takes-up-disk-usage-command-du.html](http://www.kavoir.com/2009/09/linux-check-how-much-disk-storage-each-directory-takes-up-disk-usage-command-du.html)

## Disk erweitern

Ich habe leider meine Disk immer mit LVM angelegt und wenn es um eine größere Disk ging bisher immer die komplette Maschine neu aufgesetzt.  
Wenn man nicht der Linuxprofi ist und weiterhin das einfache erweitern einer virtuellen Disk in Windows gewohnt ist, würde man am liebsten bei den ganzen verschiedenen Anleitungen und Befehlen das Internet sprengen.  
Mein Weg, der jedenfalls bei mir mit Ubuntu 18.04 funktioniert hat und relativ einfach war ist folgender.  
Eine Vorprüfung der Festplattenstruktur kann mit folgendem Befehl durchgeführt werden `sudo lsblk`  
Falls mit diesem Befehl festgestellt wurde, dass die über dem LVM liegende Disk schon erweitert wurde, kann man alle Punkte bis Punkt 7 überspringen.

1. Virtuelle Disk im Hyper-V erweitert.
2. gparted ISO geladen und als Startdisk in der VM verwendet
3. Bei der gparted Menüauswahl den Punkt mit LVM verwendet.
4. Standard Einstellungen wie Sprache und XWindow bestätigt.
5. Im grafischen Menü meine Root Disk um den entsprechenden Wert erweitert.
6. Nach der erfolgreichen Änderung wieder normal in Ubuntu gestartet.
7. Ein kurzer Check mit **df -h** zeigt aber noch die alte Größe und deshalb habe ich noch folgende zwei Befehle durchgeführt, bei denen ich nicht weiß ob beide gleichzeitig nötig waren, die aber zu einem positiven Ergebnis geführt haben. <dl><dd>`sudo lvextend -L +40G /dev/mapper/XXXX--vg-root` (Hiermit wird die Disk um 40 Gigabyte erweitert) <dl><dd>`sudo lvextend -L 40G /dev/mapper/XXXX--vg-root` (Hiermit wird die Disk auf die neue Größe von 40 Gigabyte gesetzt)</dd></dl></dd><dd>`sudo resize2fs /dev/mapper/XXXX--vg-root`</dd></dl>
8. Mit **df -h** positiv nachgeprüft ob die Disk erweitert wurde.

Quelle für Punkt 7:
https://unix.stackexchange.com/questions/664486/lvm-root-partition-only-uses-half-the-volume-size