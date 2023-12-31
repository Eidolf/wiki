# Nextcloud

# <span class="mw-headline" id="bkmrk-occ-befehle-1">OCC Befehle</span>

OCC Befehle sind auf php angewiesen und müssen als Nextcloud Service Konto ausgeführt werden.  
Die Befehle lassen sich am einfachsten ausführen wenn man sich unter dem Nextcloud Installationspfad befindet  
`cd /var/www/nextcloud`

## <span class="mw-headline" id="bkmrk-upgrade-der-nextclou-1">Upgrade der Nextcloud</span>

`sudo -u www-data php occ upgrade`

## <span class="mw-headline" id="bkmrk-upgrade-der-datenban-1">Upgrade der Datenbankstruktur</span>

`sudo -u www-data php occ db:convert-filecache-bigint`

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-wartungsmodus-aktivi-1">Wartungsmodus aktivieren/deaktivieren</span>

`sudo -u www-data php occ maintenance:mode --on`  
`sudo -u www-data php occ maintenance:mode --off`

# <span class="mw-headline" id="bkmrk-ldap-verbindung-1">LDAP Verbindung</span>

## <span class="mw-headline" id="bkmrk-ldaps-1">LDAPS</span>

<div class="vector-body" id="bkmrk-rootca-zertifikat-un"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. RootCA Zertifikat unter **/etc/ssl/certs** mit der Dateiendung .pem ablegen.
2. Die Datei ldap.conf bearbeiten `nano /etc/ldap/ldap.conf`

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://help.nextcloud.com/t/activate-ldap-over-ssl/26221" rel="nofollow">https://help.nextcloud.com/t/activate-ldap-over-ssl/26221</a>
```

## <span class="mw-headline" id="bkmrk-ldap-mit-occ-verwalt-1">LDAP mit OCC verwalten</span>

### <span class="mw-headline" id="bkmrk-ldap-einstellungen-b-1">LDAP Einstellungen betrachten</span>

`sudo -u www-data php occ ldap:show-config s03`

### <span class="mw-headline" id="bkmrk-ldap-einstellungen-s-1">LDAP Einstellungen setzen</span>

`sudo -u www-data php occ ldap:set-config s03`

#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://docs.nextcloud.com/server/15/admin_manual/configuration_server/occ_command.html#ldap-commands-label" rel="nofollow">https://docs.nextcloud.com/server/15/admin_manual/configuration_server/occ_command.html#ldap-commands-label</a>
```

# <span class="mw-headline" id="bkmrk-fehlerbehebung-1">Fehlerbehebung</span>

## <span class="mw-headline" id="bkmrk-php-speicher-512mb-1">PHP Speicher 512MB</span>

Bei einem Update auf Nextcloud Version 25 wurde bei mir weiterhin die PHP Version von CLI auf 8.1 FPM geändert.  
Hiermit hat sich mein Vorgehen etwas geändert, die Alte Version lasse ich als Archiv vorhanden.

<div class="vector-body" id="bkmrk-nano-%2Fetc%2Fphp%2F8.1%2Ffp"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `nano /etc/php/8.1/fpm/php.ini`
2. Suchen nach dem Eintrag **Strg+W** &gt; **memory\_limit**
3. Ändern des Wertes Memory Limit auf 512M `memory_limit = 512M`
4. Speichern
5. Nach dem Speichern den Service durchstarten `systemctl restart php8.1-fpm`

</div></div></div>**Alte Version**  
*Nach jedem Update wird der eingetragene Wert in der **.htaccess** Datei gelöscht (Ich habe bisher nicht überprüft ob das sinnvoller zu handhaben ist)*  
*Somit folgende Lösung nach jedem Update:*

<div class="vector-body" id="bkmrk-nano-%2Fvar%2Fwww%2Fnextcl"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `nano /var/www/nextcloud/.htaccess`
2. *Hinzufügen folgender Zeile bei den PHP Einstellungen* `php_value memory_limit 512M`
3. *Speichern*

</div></div></div>## <span class="mw-headline" id="bkmrk-update-vorgang-bleib-1">Update Vorgang bleibt stehen</span>

Es wird ein Update Vorgang gestartet, dieser bleibt im Punkt 5 hängen **Step 5 is currently in process. Please reload this page later**  
Der Status ändert sich auch nicht nach einem Neustart.  
Weiterhin wird mit den OCC Befehl angezeigt das Nextcloud auf dem neuesten Stand ist und kein Update ansteht.  
Primär kann das Problem mit einer fehlenden PHP Speicher Einstellung auf 512MB zusammenhängen.  
Bevor man daher mit der Lösung beginnt sollte sichergestellt sein das die Werte in der ".htaccess" richtig gesetzt sind.  
  
Lösung:

<div class="vector-body" id="bkmrk-den-tempor%C3%A4ren-updat"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Den Temporären Update Ordner im "data" Ordner löschen, erkennbar durch eine Zeichenreihenfolge.
2. `rm -r /var/www/nextcloud/data/updater-XXXguidXXX/`
3. Updatevorgang erneut starten.

</div></div></div># <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-php-erg%C3%A4nzungen-1">PHP Ergänzungen</span>

Nicht für jede Installation!  
Folgende Modulkette kann mit einem Befehl der unter dem Linux Eintrag zu finden ist, installiert werden.  
`{bcmath,curl,dom,gd,gmp,imagick,intl,ldap,mbstring,mysqli,mysqlnd,SimpleXML,smbclient,xml,xmlreader,xmlwriter,xsl,zip}`  
bcmath muss evtl. mit folgendem Befehl extra installiert werden &gt; `sudo apt install php7.4-bcmath`