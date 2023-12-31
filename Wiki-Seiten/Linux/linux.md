---
title: linux
description: 
published: true
date: 2024-01-03T09:53:02.649Z
tags: 
editor: markdown
dateCreated: 2024-01-03T09:53:02.649Z
---

# Linux

# Allgemein

## Bash

### Überschriebene Bash wiederherstellen

1.  Das System mit gedrückter shift Taste starten oder bei einer VM ist es besser eine Linux CD einzulegen und den Recover Mode zu starten
2.  Wenn das System mit shift gestartet wurde muss man **e** drücken und folgendes in der Zeile die mit **Linux** startet hinten einfügen `init=/bin/sh`, danach Strg+X zum speichern.
3.  Bei beidem sollte man in der Dashshell landen
4.  Als nächstes die Standard Bash umändern `chsh username -s /bin/sh`
5.  Neustart
6.  Anmelden
7.  Normale Bash neu installieren`sudo apt-get install --reinstall bash`
8.  Die Standard Bash wieder zurückändern auf die normale `sudo chsh username -s /bin/bash`

#### Quelle:

[https://askubuntu.com/questions/935528/how-do-i-recover-a-deleted-or-overwritten-bash](https://askubuntu.com/questions/935528/how-do-i-recover-a-deleted-or-overwritten-bash)

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

[https://en.wikipedia.org/wiki/Cron](https://en.wikipedia.org/wiki/Cron)

## Disk Größe

Wird der Befehl **df** verwendet  
z.B. `df` oder `df /dev/sda1`  
Um die Disk in Gigabyte anzuzeigen folgenden Befehl verwenden  
`df -BG`  
Auch ein guter Schalter ist **\-h**, hier wird die Übersicht und Größe automatisch eingestellt.  
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

1.  Virtuelle Disk im Hyper-V erweitert.
2.  gparted ISO geladen und als Startdisk in der VM verwendet
3.  Bei der gparted Menüauswahl den Punkt mit LVM verwendet.
4.  Standard Einstellungen wie Sprache und XWindow bestätigt.
5.  Im grafischen Menü meine Root Disk um den entsprechenden Wert erweitert.
6.  Nach der erfolgreichen Änderung wieder normal in Ubuntu gestartet.
7.  Ein kurzer Check mit **df -h** zeigt aber noch die alte Größe und deshalb habe ich noch folgende zwei Befehle durchgeführt, bei denen ich nicht weiß ob beide gleichzeitig nötig waren, die aber zu einem positiven Ergebnis geführt haben.
    
    `sudo lvextend -L +40G /dev/mapper/XXXX--vg-root` (Hiermit wird die Disk um 40 Gigabyte erweitert)
    
    `sudo lvextend -L 40G /dev/mapper/XXXX--vg-root` (Hiermit wird die Disk auf die neue Größe von 40 Gigabyte gesetzt)
    
    `sudo resize2fs /dev/mapper/XXXX--vg-root`
    
8.  Mit **df -h** positiv nachgeprüft ob die Disk erweitert wurde.

Quelle für Punkt 7:  
[https://unix.stackexchange.com/questions/664486/lvm-root-partition-only-uses-half-the-volume-size](https://unix.stackexchange.com/questions/664486/lvm-root-partition-only-uses-half-the-volume-size)

## Entpacken

### TAR

In einen bestimmten Ordner:  
`tar xf Datei.tar.gz -C /Zielpfad/Zielordner`

### GZIP

In einen bestimmten Ordner:  
`gunzip -c Datei.gz > /Zielpfad/Zielordner/Zieldatei.endung`

### ZIP

Benötigt das Programm unzip  
`sudo apt install unzip`  
Dann folgendermaßen um es in einen bestimmten Ordner zu entpacken:  
`unzip filename.zip -d /pfad/zum/ordner`

#### Quelle:

[https://askubuntu.com/questions/45349/how-to-extract-files-to-another-directory-using-tar-command](https://askubuntu.com/questions/45349/how-to-extract-files-to-another-directory-using-tar-command)  
[https://superuser.com/questions/139419/how-do-i-gunzip-to-a-different-destination-directory](https://superuser.com/questions/139419/how-do-i-gunzip-to-a-different-destination-directory)

## Netzwerk

### IP Einstellungen anzeigen

`ifconfig`

### Gateway auslesen

`ip route show`

### DNS Server

#### DNS Server auslesen

`systemd-resolve --status`

#### Ping zu internen Adressen schlägt fehl

Der Ping zu internen Hostnamen schlägt fehl, auch mit dem FQDN funktioniert es nicht. Ping auf die IP Adresse wiederum funktioniert und auch die Namensauflösung mit nslookup.  
Wenn unter der Konfiguration > `/etc/nsswitch.conf`  
folgendes steht > `hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4`  
Dann kann man probieren die Zeile in folgendes abzuändern und den Test erneut ausführen > `hosts: files dns`  
Wenn dadurch der Ping wieder funktioniert kann man mit folgenden Befehl **mdns** komplett entfernen > `apt-get remove libnss-mdns`

### DHCP Renew

`sudo dhclient -r`  
`sudo dhclient`

#### Quelle:

[https://www.cyberciti.biz/faq/howto-linux-renew-dhcp-client-ip-address/](https://www.cyberciti.biz/faq/howto-linux-renew-dhcp-client-ip-address/)

### netplan

In den neuesten Distributionen wird netplan verwendet, folgend eine funktionierende Beispielkonfiguration für eine Windows Domäne.  
Die Konfigurationsdatei befindet sich unter `/etc/netplan/*.yaml`

```
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.99/24]
      gateway4: 192.168.1.254
      nameservers:
        search: [domäne.local]
        addresses: [192.168.1.1,192.168.1.11]
```

Folgende Befehle ausführen:  
`netplan try`  
`netplan apply`

#### netplan Microsoft DHCP

Falls der netplan nicht geändert wird versucht Ubuntu einen DHCP Lease über eine eindeutige ID zu bekommen.  
Deshalb muss im **netplan** folgender wert eingetragen werden `dhcp-identifier: mac`  
Mit diesem Wert übergibt Ubuntu wieder die MAC Adresse an den DHCP Server und Reservierungen mit der MAC Adresse funktionieren.  
Beispieldatei:

```
network:
  ethernets:
    eth0:
      addresses: []
      dhcp4: true
      dhcp-identifier: mac
      optional: true
   version: 2
```

## swap

Swap ist für Linux das was für Windows die Auslagerungsdatei (pagefile) ist, ein virtueller Zwischenspeicher auf der Festplatte der genutzt wird um bei Arbeitsspeicherknappheit, Daten auszulagern.  
Um den Zwischenspeicher anzusehen und zu bereinigen kommen folgende Befehle zum Einsatz.

1.  `free -m` Arbeitsspeicher und Swap prüfen
2.  `swapoff -a` Swap wird deaktiviert und alles daraus wird in den Arbeitsspeicher verschoben (sollte genug vorhanden sein)
3.  `swapon -a` Swap wieder aktivieren

### <Quelle:

[https://www.redhat.com/sysadmin/clear-swap-linux](https://www.redhat.com/sysadmin/clear-swap-linux)

## sudo

### sudo su ohne Passworteingabe

Für einen Bestimmten Benutzer einstellen keine erneute Passwortanfrage zu bekommen, nötig bzw. von Vorteil für WinSCP

1.  Eine Datei unter /etc/sudoers.d anlegen
    
    `nano /etc/sudoers.d/Datei`
    
2.  Folgende Zeile eingeben und danach speichern > Benutzer ersetzen mit dem eigens verwendeten.
    
    `Benutzer ALL=(ALL) NOPASSWD:ALL`
    

#### Quelle:

[https://winscp.net/eng/docs/faq\_su](https://winscp.net/eng/docs/faq_su)

## Taskmanager

### top (Standard)

Standard Prozess Manager  
Arbeitsspeicher auslesen

### htop (Erweitert)

Muss nachinstalliert werden, ist etwas besser grafisch aufbereitet.

## Webserver verwalten

[linux-webserver-verwalten](/de/Wiki-Seiten/Serverapplikationen/Webserver/linux-webserver-verwalten)

## Version anzeigen

### Direkt ohne Zusatzsoftware

`cat /etc/issue`

### Mit Zusatzsoftware inxi

`inxi -Sz`

# Ubuntu

## Dienste

### Deaktivieren

```
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
rm /etc/systemd/system/[servicename] symlinks that might be related
systemctl daemon-reload
systemctl reset-failed
```

#### Quelle:

[https://superuser.com/questions/513159/how-to-remove-systemd-services](https://superuser.com/questions/513159/how-to-remove-systemd-services)

## Installation von Hyper-V Integrationsdiensten

Anleitung für Ubuntu 16.04

```
apt-get update
apt-get install linux-virtual-lts-xenial
```

```
apt-get install linux-tools-virtual-lts-xenial linux-cloud-tools-virtual-lts-xenial
```

### Quelle:

[https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-ubuntu-virtual-machines-on-hyper-v](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-ubuntu-virtual-machines-on-hyper-v)

## Java verwalten

Im Link wird ausführlich die Java Installation erklärt.

### Quelle:

[https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04)

## Windows Share verbinden

1.  `sudo apt-get install cifs-utils`
2.  `sudo mkdir /media/windowsshare`
3.  `sudo nano /etc/fstab`
4.  Zeile einfügen **//Dateiserver/cifs-sharename /media/windowsshare cifs username=msusername,password=mspassword,domain=msdomain,iocharset=utf8,sec=ntlm 0 0**
    
    Falls Spezielle Benutzer Rechte benötigen schaut die Zeile folgendermaßen aus
    
    **//Dateiserver/cifs-sharename /media/windowsshare cifs username=msusername,password=mspassword,domain=msdomain,uid=1000,gid=1000,file\_mode=0755,dir\_mode=0770,iocharset=utf8,sec=ntlm 0 0**
    
    -   Die gerade eingefügte Zeile ist grundsätzlich unsicher, eine sichere Methode ist in der Quelle zu finden.
5.  Speichern
6.  sudo mount -a

### Quelle:

[https://wiki.ubuntu.com/MountWindowsSharesPermanently](https://wiki.ubuntu.com/MountWindowsSharesPermanently)  
[https://wiki.ubuntuusers.de/Samba\_Client\_cifs/](https://wiki.ubuntuusers.de/Samba_Client_cifs/)

## Upgrade

Grundsätzlich ist es folgender Befehl  
`sudo do-release-upgrade`  
Doch kann es bei einer Webserver-Version danach zu Problemen kommen wenn man nicht alles beachtet. In der Quelle ist eine relativ gute Beschreibung die sich beim Upgrade-Vorgang zwar auch unterschieden hat, aber gute Hinweise gibt.

Für ein Server Upgrade das nicht auf eine Grafische Oberfläche umgestellt werden soll ist folgender Befehl gedacht  
`sudo do-release-upgrade --mode=server --allow-third-party --quiet`

### Quelle:

[https://helgeklein.com/blog/2018/12/upgrading-ubuntu-16-04-to-18-04-php-7-0-to-7-2-for-wordpress/](https://helgeklein.com/blog/2018/12/upgrading-ubuntu-16-04-to-18-04-php-7-0-to-7-2-for-wordpress/)