---
title: linux
description: 
published: true
date: 2024-01-02T15:30:56.385Z
tags: 
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

## <span class="mw-headline" id="bkmrk-berechtigung-1">Berechtigung</span>

### <span class="mw-headline" id="bkmrk-berechtigung-auslese-1">Berechtigung auslesen</span>

Für einen Ordner mit den darin liegenden Dateien  
`ls -al /Ordnerpfad`

### <span class="mw-headline" id="bkmrk-gruppenberechtigung-1">Gruppenberechtigung</span>

#### <span class="mw-headline" id="bkmrk-gruppenberechtigung--1">Gruppenberechtigung auslesen</span>

Man gibt den Benutzer an von dem man die Gruppenzugehörigkeit auslesen möchte  
`groups Benutzer`

#### <span class="mw-headline" id="bkmrk-berechtigung-auf-gru-1">Berechtigung auf Gruppe erteilen</span>

`sudo useradd -G Gruppe Benutzer`

#### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-berechtigung-auf-gru-3">Berechtigung auf Gruppe löschen</span>

`sudo gpasswd --delete Benutzer Gruppe`

## <span class="mw-headline" id="bkmrk-cronjob-1">Cronjob</span>

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

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://en.wikipedia.org/wiki/Cron" rel="nofollow">https://en.wikipedia.org/wiki/Cron</a>
```

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-disk-gr%C3%B6%C3%9Fe-1">Disk Größe</span>

Wird der Befehl **df** verwendet  
z.B. `df` oder `df /dev/sda1`  
Um die Disk in Gigabyte anzuzeigen folgenden Befehl verwenden  
`df -BG`  
Auch ein guter Schalter ist **-h**, hier wird die Übersicht und Größe automatisch eingestellt.  
`df -h`

## <span id="bkmrk--3"></span><span class="mw-headline" id="bkmrk-ordner-gr%C3%B6%C3%9Fe-1">Ordner Größe</span>

Wird der Befehl **du** verwendet  
Um z.B. für einen Ordner die Gesamtgröße herauszufinden  
`du -sh /var/lib/Ordner`

### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

[http://www.kavoir.com/2009/09/linux-check-how-much-disk-storage-each-directory-takes-up-disk-usage-command-du.html](http://www.kavoir.com/2009/09/linux-check-how-much-disk-storage-each-directory-takes-up-disk-usage-command-du.html)

## <span class="mw-headline" id="bkmrk-disk-erweitern-1">Disk erweitern</span>

Ich habe leider meine Disk immer mit LVM angelegt und wenn es um eine größere Disk ging bisher immer die komplette Maschine neu aufgesetzt.  
Wenn man nicht der Linuxprofi ist und weiterhin das einfache erweitern einer virtuellen Disk in Windows gewohnt ist, würde man am liebsten bei den ganzen verschiedenen Anleitungen und Befehlen das Internet sprengen.  
Mein Weg, der jedenfalls bei mir mit Ubuntu 18.04 funktioniert hat und relativ einfach war ist folgender.  
Eine Vorprüfung der Festplattenstruktur kann mit folgendem Befehl durchgeführt werden `sudo lsblk`  
Falls mit diesem Befehl festgestellt wurde, dass die über dem LVM liegende Disk schon erweitert wurde, kann man alle Punkte bis Punkt 7 überspringen.

<div class="vector-body" id="bkmrk-virtuelle-disk-im-hy"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Virtuelle Disk im Hyper-V erweitert.
2. gparted ISO geladen und als Startdisk in der VM verwendet
3. Bei der gparted Menüauswahl den Punkt mit LVM verwendet.
4. Standard Einstellungen wie Sprache und XWindow bestätigt.
5. Im grafischen Menü meine Root Disk um den entsprechenden Wert erweitert.
6. Nach der erfolgreichen Änderung wieder normal in Ubuntu gestartet.
7. Ein kurzer Check mit **df -h** zeigt aber noch die alte Größe und deshalb habe ich noch folgende zwei Befehle durchgeführt, bei denen ich nicht weiß ob beide gleichzeitig nötig waren, die aber zu einem positiven Ergebnis geführt haben. <dl><dd>`sudo lvextend -L +40G /dev/mapper/XXXX--vg-root` (Hiermit wird die Disk um 40 Gigabyte erweitert) <dl><dd>`sudo lvextend -L 40G /dev/mapper/XXXX--vg-root` (Hiermit wird die Disk auf die neue Größe von 40 Gigabyte gesetzt)</dd></dl></dd><dd>`sudo resize2fs /dev/mapper/XXXX--vg-root`</dd></dl>
8. Mit **df -h** positiv nachgeprüft ob die Disk erweitert wurde.

</div></div></div>Quelle für Punkt 7:

```
<a class="external free" href="https://unix.stackexchange.com/questions/664486/lvm-root-partition-only-uses-half-the-volume-size" rel="nofollow">https://unix.stackexchange.com/questions/664486/lvm-root-partition-only-uses-half-the-volume-size</a>
```

## <span class="mw-headline" id="bkmrk-entpacken-1">Entpacken</span>

### <span class="mw-headline" id="bkmrk-tar-1">TAR</span>

In einen bestimmten Ordner:  
`tar xf Datei.tar.gz -C /Zielpfad/Zielordner`

### <span class="mw-headline" id="bkmrk-gzip-1">GZIP</span>

In einen bestimmten Ordner:  
`gunzip -c Datei.gz > /Zielpfad/Zielordner/Zieldatei.endung`

### <span class="mw-headline" id="bkmrk-zip-1">ZIP</span>

Benötigt das Programm unzip  
`sudo apt install unzip`  
Dann folgendermaßen um es in einen bestimmten Ordner zu entpacken:  
`unzip filename.zip -d /pfad/zum/ordner`

#### <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="https://askubuntu.com/questions/45349/how-to-extract-files-to-another-directory-using-tar-command" rel="nofollow">https://askubuntu.com/questions/45349/how-to-extract-files-to-another-directory-using-tar-command</a>
<a class="external free" href="https://superuser.com/questions/139419/how-do-i-gunzip-to-a-different-destination-directory" rel="nofollow">https://superuser.com/questions/139419/how-do-i-gunzip-to-a-different-destination-directory</a>
```

## <span class="mw-headline" id="bkmrk-netzwerk-1">Netzwerk</span>

### <span class="mw-headline" id="bkmrk-ip-einstellungen-anz-1">IP Einstellungen anzeigen</span>

`ifconfig`

### <span class="mw-headline" id="bkmrk-gateway-auslesen-1">Gateway auslesen</span>

`ip route show`

### <span class="mw-headline" id="bkmrk-dns-server-1">DNS Server</span>

#### <span class="mw-headline" id="bkmrk-dns-server-auslesen-1">DNS Server auslesen</span>

`systemd-resolve --status`

#### <span id="bkmrk--4"></span><span class="mw-headline" id="bkmrk-ping-zu-internen-adr-1">Ping zu internen Adressen schlägt fehl</span>

Der Ping zu internen Hostnamen schlägt fehl, auch mit dem FQDN funktioniert es nicht. Ping auf die IP Adresse wiederum funktioniert und auch die Namensauflösung mit nslookup.  
Wenn unter der Konfiguration &gt; `/etc/nsswitch.conf`  
folgendes steht &gt; `hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4`  
Dann kann man probieren die Zeile in folgendes abzuändern und den Test erneut ausführen &gt; `hosts: files dns`  
Wenn dadurch der Ping wieder funktioniert kann man mit folgenden Befehl **mdns** komplett entfernen &gt; `apt-get remove libnss-mdns`

### <span class="mw-headline" id="bkmrk-dhcp-renew-1">DHCP Renew</span>

`sudo dhclient -r`  
`sudo dhclient`

#### <span class="mw-headline" id="bkmrk-quelle%3A-9">Quelle:</span>

```
<a class="external free" href="https://www.cyberciti.biz/faq/howto-linux-renew-dhcp-client-ip-address/" rel="nofollow">https://www.cyberciti.biz/faq/howto-linux-renew-dhcp-client-ip-address/</a>
```

### <span class="mw-headline" id="bkmrk-netplan-1">netplan</span>

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

#### <span class="mw-headline" id="bkmrk-netplan-microsoft-dh-1">netplan Microsoft DHCP</span>

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

## <span class="mw-headline" id="bkmrk-swap-1">swap</span>

Swap ist für Linux das was für Windows die Auslagerungsdatei (pagefile) ist, ein virtueller Zwischenspeicher auf der Festplatte der genutzt wird um bei Arbeitsspeicherknappheit, Daten auszulagern.  
Um den Zwischenspeicher anzusehen und zu bereinigen kommen folgende Befehle zum Einsatz.

<div class="vector-body" id="bkmrk-free--m%C2%A0arbeitsspeic"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `free -m` Arbeitsspeicher und Swap prüfen
2. `swapoff -a` Swap wird deaktiviert und alles daraus wird in den Arbeitsspeicher verschoben (sollte genug vorhanden sein)
3. `swapon -a` Swap wieder aktivieren

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-11">Quelle:</span>

```
<a class="external free" href="https://www.redhat.com/sysadmin/clear-swap-linux" rel="nofollow">https://www.redhat.com/sysadmin/clear-swap-linux</a>
```

## <span class="mw-headline" id="bkmrk-sudo-1">sudo</span>

### <span class="mw-headline" id="bkmrk-sudo-su-ohne-passwor-1">sudo su ohne Passworteingabe</span>

Für einen Bestimmten Benutzer einstellen keine erneute Passwortanfrage zu bekommen, nötig bzw. von Vorteil für WinSCP

<div class="vector-body" id="bkmrk-eine-datei-unter-%2Fet"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Eine Datei unter /etc/sudoers.d anlegen <dl><dd>`nano /etc/sudoers.d/Datei`</dd></dl>
2. Folgende Zeile eingeben und danach speichern &gt; Benutzer ersetzen mit dem eigens verwendeten. <dl><dd>`Benutzer ALL=(ALL) NOPASSWD:ALL`</dd></dl>

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-13">Quelle:</span>

```
<a class="external free" href="https://winscp.net/eng/docs/faq_su" rel="nofollow">https://winscp.net/eng/docs/faq_su</a>
```

## <span class="mw-headline" id="bkmrk-taskmanager-1">Taskmanager</span>

### <span id="bkmrk--5"></span><span class="mw-headline" id="bkmrk-top-%28standard%29-1">top (Standard)</span>

Standard Prozess Manager  
Arbeitsspeicher auslesen

### <span id="bkmrk--6"></span><span class="mw-headline" id="bkmrk-htop-%28erweitert%29-1">htop (Erweitert)</span>

Muss nachinstalliert werden, ist etwas besser grafisch aufbereitet.

## <span class="mw-headline" id="bkmrk-webserver-verwalten-1">Webserver verwalten</span>

[Linux Webserver verwalten](https://wiki.eidolf.de/index.php/Linux_Webserver_verwalten "Linux Webserver verwalten")

## <span class="mw-headline" id="bkmrk-version-anzeigen-1">Version anzeigen</span>

### <span class="mw-headline" id="bkmrk-direkt-ohne-zusatzso-1">Direkt ohne Zusatzsoftware</span>

`cat /etc/issue`

### <span class="mw-headline" id="bkmrk-mit-zusatzsoftware-i-1">Mit Zusatzsoftware inxi</span>

`inxi -Sz`

# <span class="mw-headline" id="bkmrk-ubuntu-1">Ubuntu</span>

## <span class="mw-headline" id="bkmrk-dienste-1">Dienste</span>

### <span class="mw-headline" id="bkmrk-deaktivieren-1">Deaktivieren</span>

```
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
rm /etc/systemd/system/[servicename] symlinks that might be related
systemctl daemon-reload
systemctl reset-failed
```

#### <span class="mw-headline" id="bkmrk-quelle%3A-15">Quelle:</span>

```
<a class="external free" href="https://superuser.com/questions/513159/how-to-remove-systemd-services?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa" rel="nofollow">https://superuser.com/questions/513159/how-to-remove-systemd-services?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa</a>
```

## <span class="mw-headline" id="bkmrk-installation-von-hyp-1">Installation von Hyper-V Integrationsdiensten</span>

Anleitung für Ubuntu 16.04

```
apt-get update
apt-get install linux-virtual-lts-xenial
```

```
apt-get install linux-tools-virtual-lts-xenial linux-cloud-tools-virtual-lts-xenial
```

### <span class="mw-headline" id="bkmrk-quelle%3A-17">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-ubuntu-virtual-machines-on-hyper-v" rel="nofollow">https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-ubuntu-virtual-machines-on-hyper-v</a>
```

## <span class="mw-headline" id="bkmrk-java-verwalten-1">Java verwalten</span>

Im Link wird ausführlich die Java Installation erklärt.

### <span class="mw-headline" id="bkmrk-quelle%3A-19">Quelle:</span>

```
<a class="external free" href="https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04" rel="nofollow">https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04</a>
```

## <span class="mw-headline" id="bkmrk-windows-share-verbin-1">Windows Share verbinden</span>

<div class="vector-body" id="bkmrk-sudo-apt-get-install-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `sudo apt-get install cifs-utils`
2. `sudo mkdir /media/windowsshare`
3. `sudo nano /etc/fstab`
4. Zeile einfügen **//Dateiserver/cifs-sharename /media/windowsshare cifs username=msusername,password=mspassword,domain=msdomain,iocharset=utf8,sec=ntlm 0 0**<dl><dd><dl><dd>Falls Spezielle Benutzer Rechte benötigen schaut die Zeile folgendermaßen aus</dd><dd>**//Dateiserver/cifs-sharename /media/windowsshare cifs username=msusername,password=mspassword,domain=msdomain,uid=1000,gid=1000,file\_mode=0755,dir\_mode=0770,iocharset=utf8,sec=ntlm 0 0**</dd></dl></dd></dl>
    - Die gerade eingefügte Zeile ist grundsätzlich unsicher, eine sichere Methode ist in der Quelle zu finden.
5. Speichern
6. sudo mount -a

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-21">Quelle:</span>

```
<a class="external free" href="https://wiki.ubuntu.com/MountWindowsSharesPermanently" rel="nofollow">https://wiki.ubuntu.com/MountWindowsSharesPermanently</a>
<a class="external free" href="https://wiki.ubuntuusers.de/Samba_Client_cifs/" rel="nofollow">https://wiki.ubuntuusers.de/Samba_Client_cifs/</a>
```

## <span class="mw-headline" id="bkmrk-upgrade-1">Upgrade</span>

Grundsätzlich ist es folgender Befehl  
`sudo do-release-upgrade`  
Doch kann es bei einer Webserver-Version danach zu Problemen kommen wenn man nicht alles beachtet. In der Quelle ist eine relativ gute Beschreibung die sich beim Upgrade-Vorgang zwar auch unterschieden hat, aber gute Hinweise gibt.  
  
Für ein Server Upgrade das nicht auf eine Grafische Oberfläche umgestellt werden soll ist folgender Befehl gedacht  
`sudo do-release-upgrade --mode=server --allow-third-party --quiet`

### <span class="mw-headline" id="bkmrk-quelle%3A-23">Quelle:</span>

```
<a class="external free" href="https://helgeklein.com/blog/2018/12/upgrading-ubuntu-16-04-to-18-04-php-7-0-to-7-2-for-wordpress/" rel="nofollow">https://helgeklein.com/blog/2018/12/upgrading-ubuntu-16-04-to-18-04-php-7-0-to-7-2-for-wordpress/</a>
```