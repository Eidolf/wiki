---
title: Nextcloud
description: 
published: true
date: 2025-10-29T16:45:28.488Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:36:29.130Z
---

# OCC Befehle

OCC Befehle sind auf php angewiesen und müssen als Nextcloud Service Konto ausgeführt werden.  
Die Befehle lassen sich am einfachsten ausführen wenn man sich unter dem Nextcloud Installationspfad befindet  
`cd /var/www/nextcloud`

## Upgrade der Nextcloud

`sudo -u www-data php occ upgrade`

### Wenn die Webseite ein Update anzeigt aber OCC nicht
Eigentlich ist das der normal Update Befehl wie auf der Webseite. 
Das **--no-interaction** bewirkt nicht noch nachträglich den **occ upgrade** ausführen zu müssen.

`sudo -u www-data php /var/www/nextcloud/updater/updater.phar --no-interaction` 

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
### Bei Punkt 3
Bei der Prüfung der **write permissions** wurde die package-lock.json erwähnt, dass hierauf kein Schreibzugriff möglich ist.
Ich habe mir dann unter
`/var/www/nextcloud`
alle Rechte angeschaut und gemerkt das die Datei mit root berechtigt war.
Daher eine Änderung mit folgendem Befehl in dem Ordner durchgeführt.
`chown www-data:www-data package-lock.json`

### Bei Punkt 5
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

# Sicherheits Verbesserungen
## Nextcloud: PHP OPcache – Warnung „interned strings buffer fast voll“ beheben

Nextcloud kann folgende Meldung anzeigen:

> Das PHP OPcache-Modul ist nicht ordnungsgemäß konfiguriert. Der „OPcache interned strings“-Puffer ist fast voll. Um sicherzustellen, dass sich wiederholende Strings effektiv zwischengespeichert werden können, wird empfohlen, `opcache.interned_strings_buffer` mit einem Wert größer als `8` zu setzen.

Ziel: `opcache.interned_strings_buffer` (und ggf. weitere OPcache-Werte) erhöhen und den Webserver/PHP-Dienst neu starten.


### Schritt 1: PHP-Version ermitteln

```php -v```

Beispiel Ausgabe:
PHP 8.1.2 (cli) (built: ...)


> **Hinweis:** Die für **Apache** (mod_php) oder **PHP-FPM** genutzte PHP-Version kann sich von der CLI-Version unterscheiden. Entscheidend ist die Version/`php.ini`, die dein Webserver für Nextcloud verwendet.


### Schritt 2: Richtige `php.ini`-Datei finden

Je nach Setup (ersetze `8.1` durch deine Version):

- **Apache (mod_php):**
/etc/php/8.1/apache2/php.ini

- **PHP-FPM (häufig bei Apache mit Proxy oder Nginx):**
/etc/php/8.1/fpm/php.ini

- **CLI (nur zur Anzeige über `php -i`, nicht für Web relevant):**
/etc/php/8.1/cli/php.ini

> **Wichtig:** Ändere **nicht** nur die CLI-`php.ini`, wenn Nextcloud über Apache/FPM läuft. Passe die Datei der **laufenden SAPI** (apache2 oder fpm) an.


### Schritt 3: OPcache-Einstellungen anpassen

Datei mit Root-Rechten öffnen (Beispiel Apache):

`sudo nano /etc/php/8.1/apache2/php.ini`

Im Abschnitt `[opcache]` folgende Werte setzen bzw. ergänzen. Mindestens relevant für die Warnung ist `opcache.interned_strings_buffer`; sinnvoll ist, gleich ein *empfohlenes* Set zu setzen:

```
[opcache]
; OPcache aktivieren
opcache.enable=1
; Optional: auch für CLI aktivieren (praktisch für Wartungsskripte)
opcache.enable_cli=1

; Speichergrößen und Tabellen
opcache.memory_consumption=128
opcache.interned_strings_buffer=16
opcache.max_accelerated_files=10000

; Kommentare (für Annotations) erhalten
opcache.save_comments=1

; Wie oft nach Änderungen gesucht wird (Sekunden)
opcache.revalidate_freq=60
```

> **Tipp:** Wenn die Warnung weiterhin erscheint (große Installationen, viele Apps), erhöhe testweise  
> `opcache.interned_strings_buffer` auf **32** und ggf. `opcache.memory_consumption` auf **192–256**.
{.is-info}



### Schritt 4: Dienst neu starten

- **Apache (mod_php):**
`sudo systemctl restart apache2`

- **PHP-FPM (Version anpassen):**
`sudo systemctl restart php8.1-fpm`

> Falls du Apache **mit** PHP-FPM nutzt, **beide** neu starten: erst `php8.1-fpm`, dann `apache2`.


### Schritt 5: Wirksamkeit prüfen

#### Schnelltest per CLI (nur Anhaltspunkt)

`php -i | grep -E 'opcache\.enable|interned_strings_buffer|max_accelerated_files|memory_consumption|save_comments|revalidate_freq'`
> Achtung: Das zeigt die **CLI**-Werte, nicht zwingend die Web-Konfiguration.

#### Über die Nextcloud GUI

Prüfen ob der Fehler in der Nextcloud verschwunden ist.

### Häufige Stolperfallen

- **Falsche `php.ini`** angepasst (CLI statt apache2/fpm).  
- **Dienst nicht neu gestartet**, Änderungen greifen erst danach.  
- **Mehrere PHP-Versionen installiert** (8.1 und 8.2); die aktive Version für den Webserver prüfen.  
- **OPcache deaktiviert** (`opcache.enable=0`), dann greifen die anderen Werte nicht.


### Quelle:

- Nextcloud Admin Manual → *Server tuning → Enable PHP OPcache*  
https://docs.nextcloud.com/server/30/admin_manual/installation/server_tuning.html#enable-php-opcache