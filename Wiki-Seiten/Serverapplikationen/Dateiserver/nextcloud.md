---
title: nextcloud
description: 
published: true
date: 2025-10-29T10:49:50.209Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:36:29.130Z
---

# Nextcloud

# OCC Befehle

OCC Befehle sind auf php angewiesen und müssen als Nextcloud Service Konto ausgeführt werden.  
Die Befehle lassen sich am einfachsten ausführen wenn man sich unter dem Nextcloud Installationspfad befindet  
`cd /var/www/nextcloud`

## Upgrade der Nextcloud

`sudo -u www-data php occ upgrade`

## Upgrade der Datenbankstruktur

`sudo -u www-data php occ db:convert-filecache-bigint`

## Aktualisierung der Indizes

`sudo -u www-data php occ db:add-missing-indices`

## Wartungsmodus aktivieren/deaktivieren

`sudo -u www-data php occ maintenance:mode --on`  
`sudo -u www-data php occ maintenance:mode --off`

# LDAP Verbindung

## LDAPS

1. RootCA Zertifikat unter **/etc/ssl/certs** mit der Dateiendung .pem ablegen.
2. Die Datei ldap.conf bearbeiten `nano /etc/ldap/ldap.conf`

### Quelle:

https://help.nextcloud.com/t/activate-ldap-over-ssl/26221

## LDAP mit OCC verwalten

### LDAP Einstellungen betrachten

`sudo -u www-data php occ ldap:show-config s03`

### LDAP Einstellungen setzen

`sudo -u www-data php occ ldap:set-config s03`

#### Quelle:

https://docs.nextcloud.com/server/15/admin_manual/configuration_server/occ_command.html#ldap-commands-label

# Fehlerbehebung

## PHP Speicher 512MB

Bei einem Update auf Nextcloud Version 25 wurde bei mir weiterhin die PHP Version von CLI auf 8.1 FPM geändert.  
Hiermit hat sich mein Vorgehen etwas geändert, die Alte Version lasse ich als Archiv vorhanden.

1. `nano /etc/php/8.1/fpm/php.ini`
2. Suchen nach dem Eintrag **Strg+W** &gt; **memory\_limit**
3. Ändern des Wertes Memory Limit auf 512M `memory_limit = 512M`
4. Speichern
5. Nach dem Speichern den Service durchstarten `systemctl restart php8.1-fpm`

**Alte Version**  
*Nach jedem Update wird der eingetragene Wert in der **.htaccess** Datei gelöscht (Ich habe bisher nicht überprüft ob das sinnvoller zu handhaben ist)*  
*Somit folgende Lösung nach jedem Update:*

1. `nano /var/www/nextcloud/.htaccess`
2. *Hinzufügen folgender Zeile bei den PHP Einstellungen* `php_value memory_limit 512M`
3. *Speichern*

## Update Vorgang bleibt stehen

Es wird ein Update Vorgang gestartet, dieser bleibt im Punkt 5 hängen **Step 5 is currently in process. Please reload this page later**  
Der Status ändert sich auch nicht nach einem Neustart.  
Weiterhin wird mit den OCC Befehl angezeigt das Nextcloud auf dem neuesten Stand ist und kein Update ansteht.  
Primär kann das Problem mit einer fehlenden PHP Speicher Einstellung auf 512MB zusammenhängen.  
Bevor man daher mit der Lösung beginnt sollte sichergestellt sein das die Werte in der ".htaccess" richtig gesetzt sind.  
  
Lösung:

1. Den Temporären Update Ordner im "data" Ordner löschen, erkennbar durch eine Zeichenreihenfolge.
2. `rm -r /var/www/nextcloud/data/updater-XXXguidXXX/`
3. Updatevorgang erneut starten.

# PHP Ergänzungen

Nicht für jede Installation!  
Folgende Modulkette kann mit einem Befehl der unter dem Linux Eintrag zu finden ist, installiert werden.  
`{bcmath,curl,dom,gd,gmp,imagick,intl,ldap,mbstring,mysqli,mysqlnd,SimpleXML,smbclient,xml,xmlreader,xmlwriter,xsl,zip}`  
bcmath muss evtl. mit folgendem Befehl extra installiert werden &gt; `sudo apt install php7.4-bcmath`