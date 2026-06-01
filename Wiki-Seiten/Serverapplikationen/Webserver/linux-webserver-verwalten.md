---
title: linux-webserver-verwalten
description: 
published: true
date: 2026-06-01T09:48:28.749Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:36:59.517Z
---

# Erstkonfiguration

1. Voraussetzung: Ubuntu mit LAMP installiert
2. Unter dem Pfad `/etc/apache2/sites-available` eine neue Konfiguration anlegen mit untenstehenden VirtualHost Inhalt. Diese Datei muss unbedingt mit **.conf** enden, z.B. **MEIN\_Irgendwas.conf**
3. In dem angegebenen DocumentRoot Pfad sollte die gewünschte Anwendung liegen
4. Mit `sudo a2ensite MEIN_Irgendwas.conf` den virtual Host aktivieren. 
    - Hinweis: Mit **a2dissite** kann der Host wieder deaktiviert werden.
5. Folgende PHP-Module sind für viele Anwendungen zu empfehlen `apt-get install php-imap;php-xml;php-gd;php-mbstring;php-intl;php-apcu;php-ldap`

```
<VirtualHost irgendwas.eidolf.de:80>
	ServerAdmin E-Mail
	ServerName irgendwas.eidolf.de
	DocumentRoot /var/www/joomla
	ErrorLog ${APACHE_LOG_DIR}/osticket-error.log
        CustomLog ${APACHE_LOG_DIR}/osticket-access.log combined
</VirtualHost>
```

## Quelle:
https://wiki.ubuntuusers.de/Apache/Virtual_Hosts/

## SSL Konfiguration

1. Selbstsignierte Zertifikate ausstellen

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/NameVirtualWebSite-selfsigned.key -out /etc/ssl/certs/NameVirtualWebSite-selfsigned.crt`  
Ab jetzt werden ein paar Abfragen zum Zertifikat gestellt, diese einfach ausfüllen.

2. Einen starken Verschlüsselungsalgorythmus erstellen.
`sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048`

3. SSL Konfiguration erstellen
`sudo nano /etc/apache2/conf-available/ssl-params.conf`  

  Inhalt: 
```
# from https://cipherli.st/
# and https://raymii.org/s/tutorials/Strong_SSL_Security_On_Apache2.html

SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
SSLProtocol All -SSLv2 -SSLv3
SSLHonorCipherOrder On
# Disable preloading HSTS for now.  You can use the commented out header line that includes
# the "preload" directive if you understand the implications.
#Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains"
Header always set X-Frame-Options DENY
Header always set X-Content-Type-Options nosniff
# Requires Apache >= 2.4
SSLCompression off 
SSLSessionTickets Off
SSLUseStapling on 
SSLStaplingCache "shmcb:logs/stapling-cache(150000)"

SSLOpenSSLConfCmd DHParameters "/etc/ssl/certs/dhparam.pem"
```

4. SSL Virtual Host File anlegen (kann auch in das Port 80 Virtual Host File eingefügt werden)
`sudo nano /etc/apache2/sites-available/NameVirtualWebSite-ssl.conf`  

Inhalt: (Nur ein Beispiel, kann verbessert werden) 
```
<VirtualHost *:443>
    DocumentRoot /var/www/your-domain-root
    ServerName your-domain.com
    SSLEngine On
    SSLOptions +StrictRequire
    SSLCertificateFile /path/to/server.crt
    SSLCertificateKeyFile /path/to/server.key
    SSLProtocol TLSv1
</VirtualHost>
```

5. SSL in Apache aktivieren
```
sudo a2enmod ssl
sudo a2enmod headers
```

6. Apache neu starten
`service apache2 restart`

7. Site aktivieren
`sudo a2ensite NameVirtualWebSite-ssl`

8. SSL Konfiguration aktivieren
`sudo a2enconf ssl-params`

9. Apache Konfiguration neu laden
`service apache2 reload`

### Quelle:
https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04"

# Dateiupload

1. Mit WinSCP den Webserver öffnen und die Dateien in den Zielordner schieben
2. Mit einem FTP Programm in den Zielordner schieben (falls FTP Server Software am Zielsystem installiert ist)
3. Mit SSH auf den Server verbinden und mit `wget <a class="external free" href="http://link_zur_software/" rel="nofollow">http://Link_zur_Software</a>`

# httpd config bearbeiten

Mit **vi** oder **nano** die httpd.conf öffnen  
`sudo vi /etc/httpd/conf/httpd.conf`

# PHP

## php.ini bearbeiten

### Pfad zu den Dateien

`/etc/php/7.x/apache2/php.ini`  
`/etc/php/7.x/cli/php.ini`

## PHP Upgrade

### PHP PPA Repository hinzufügen

```
sudo apt-get update
sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
```

### Aktives Apache‑PHP‑Modul automatisch ermitteln

**Befehl**
``` 
apache2ctl -M | grep php
```

Zeigt z.B. folgendes an:
`php7_module (shared)`

Hier kann es noch etwas umständlich werden, da Module auch manuell in Library Dateien hinterlegt sein können.
Diese können folgendermaßen angezeigt werden.
```
ls -l /usr/lib/apache2/modules/ | grep php
```
Etwas genauer um die Dateien zu sehen.
```
grep -R "libphp" /etc/apache2
```
Diese Datei mit nano bearbeiten, also in folgendem Beispiel die Datei.
Hier einfach die Zeile auskommentieren.
```
sudo nano /etc/apache2/mods-available/php7.4.load
```

Das wird später automatisch deaktiviert.

### Installierte PHP‑Versionen anzeigen

**Befehl**
```
update-alternatives --list php
```

Zeigt z.B. folgendes an:

```
/usr/bin/php7.4
/usr/bin/php8.1
/usr/bin/php8.5
```

### Installierte Module der alten Version automatisch ermitteln

Wenn man von **8.1 → 8.5** upgraden möchte.

Liste der installierten **8.1‑Module**:

**Befehl**
```
dpkg -l | grep php8.1
```

### Automatisch dieselben Module für PHP 8.5 installieren

Das ist der wichtigste Schritt — und der sauberste.

**Befehl**
```
dpkg -l | grep php8.1 | awk '{print $2}' | sed 's/8.1/8.5/' | xargs sudo apt install -y
```

Damit werden alle Module **1:1 übernommen**, ohne selbständig die einzelnen Module angeben zu müssen.


### Apache‑PHP‑Modul automatisch deaktivieren

**Befehl**
```
sudo a2dismod $(apache2ctl -M | grep -oP 'php[0-9]+' | head -n1)
```

Das deaktiviert automatisch:

**Code**
```
php7
```

### Neues Apache‑PHP‑Modul aktivieren (8.5)

Vorher prüfen:

**Code**
```
ls /etc/apache2/mods-available/ | grep php
```

Dann aktivieren:

**Code**
```
sudo a2enmod php8.5
sudo systemctl restart apache2
```

### CLI‑PHP auf 8.5 umstellen

**Code**
```
sudo update-alternatives --set php /usr/bin/php8.5
```

Prüfen:

**Code**
```
php -v
```

### Nextcloud auf neue PHP‑Version umstellen
> Nextcloud läuft zum Stand 06.2026 nur mit php Version 8.4
{.is-warning}


Nextcloud nutzt entweder **Apache‑mod_php** oder **PHP‑FPM**.

Prüfen, ob FPM läuft:

**Code**
```
systemctl status php8.1-fpm
```

Wenn FPM aktiv ist, musst du auch **8.5‑FPM** aktivieren:
**Installieren**
```
sudo apt install php8.5-fpm
```

**Konfigurieren**
```
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php8.5-fpm
```

**Aktivieren**
```
sudo systemctl enable --now php8.5-fpm
sudo systemctl disable --now php8.1-fpm
```

Apache‑Konfiguration prüfen:

**Code**
```
ls /etc/apache2/sites-enabled/ | xargs grep -R "php8"
```

Falls du FPM nutzt, musst du den Socket anpassen:

**Code**
```
/run/php/php8.5-fpm.sock
```

### Apache neu starten

**Code**
```
sudo systemctl restart apache2
```

### Nextcloud PHP‑Version prüfen

Im Webinterface unter:

**Administration → System**

Oder per CLI:

**Code**
```
sudo -u www-data php8.5 /var/www/nextcloud/occ status
```

# Webdienst neu starten

`sudo service httpd restart`

# Zertifikat von Windows CA ausstellen

1. Ordner **anlegen etc/apache2/ssl.**<dl><dd>`cd /etc/apache2/ssl`</dd></dl>
2. Dann den Private Key erstellen: <dl><dd>`openssl genrsa 4096 > server.key`</dd></dl>
3. ...und die Zertifikats-Anforderung: <dl><dd>`openssl req -new -sha256 -key ./server.key > request.csr`</dd><dd>die Abfragen wie Ort, Servername usw. entsprechend ausfüllen.</dd></dl>
4. Request.csr ausgeben: <dl><dd>`cat request.csr`</dd><dd>Nun den gesamten Inhalt zwischen "-----BEGIN CERTIFICATE REQUEST-----" und "-----END CERTIFICATE REQUEST-----" in die #:Zwischenablage kopieren. Den brauchen wir nachher bei der Zertifikatsanfoderung.</dd></dl>
5. Webserver Zertifikat an der Windows CA ausstellen mit dem Hashwert
6. Am Linuxserver noch den Apache konfigurieren <dl><dd>`nano /etc/apache2/sites-available/default-ssl`</dd><dd>**SSLCertificateFile /etc/apache2/ssl/server.cer**</dd><dd>**SSLCertificateKeyFile /etc/apache2/ssl/server.key**</dd></dl>
7. Den Zertifikatsordner würde ich vorsichtshalber noch auf die Berechtigung 600 setzen: <dl><dd>`chmod 600 --recursive /etc/apache2/ssl`</dd></dl>
8. Danach noch die Konfiguration vom Apache neu einlesen: <dl><dd>`/etc/init.d/apache2 reload`</dd></dl>

## Quelle:

https://itwelt.org/anleitungen-howto/linux-740817696/558-apache-ssl-zertifikat-ueber-eine-windows-pki-anfordern