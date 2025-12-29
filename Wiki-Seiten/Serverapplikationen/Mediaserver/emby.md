---
title: Emby
description: 
published: true
date: 2025-12-29T16:24:10.007Z
tags: medien, media, video, musik, music, film, bilder, pictures
editor: markdown
dateCreated: 2023-12-31T13:36:42.903Z
---

# Zertifikate

## LetsEncrypt

### Erstes Zertifikat ausstellen

Dieses Beispiel wird an einem Ubuntu 16.04 Server durchgeführt.

1. Installation von Certbot siehe [Certbot Website](https://certbot.eff.org/#ubuntuxenial-other)
2. Port 80 an Emby Server freischalten bzw. über Firewall weiterleiten
3. `letsencrypt certonly --standalone -d beispiel.domain.de`
4. Wechseln zum Ordner in dem das Zertifikat ausgestellt wurde **/etc/letsencrypt/live/beispiel.domain.de/**
5. In dem Ordner gibt es 4 Dateien **cert.pem** **chain.pem** **fullchain.pem** **privkey.pem**, diese **NICHT** löschen da diese zum erneuern benötigt werden.
6. `openssl pkcs12 -inkey privkey.pem -in fullchain.pem -export -out /var/lib/emby-server/ssl/beispiel.domain.de.pfx -passout pass:`
7. In der Emby Server Verwaltung das erstellte Zertifikat auswählen.
8. Emby Server neu starten.

#### Quelle:

https://emby.media/community/index.php?/topic/42315-creating-a-letsencrypt-ssl-certificate-for-emby/

### Automatisch Update Script

Da das Script in der Quelle nicht mehr zu finden ist poste ich es hier komplett rein, es ist nicht mein Werk und falls der Urheber das Posten anmerkt werde ich es komplett löschen.

1. Installation von bc `apt-get install bc`
2. Script

```
#!/bin/bash

SSLPORT="8920"
HOST="External Emby FQDN"
RENEWDAY="60"
#CMDCERTBOT="/etc/certbot/"
CMDCERTBOT="/etc/letsencrypt/"
#CMDRENEW=$CMDCERTBOT"certbot-auto renew --standalone --force-renewal"
CMDRENEW=$CMDCERTBOT"certbot renew --standalone --force-renewal --preferred-challenges http"
CMDLETSENCRYPT="/etc/letsencrypt/"
CMDSSLDEST="/media/windowsshare/Programme/Emby/Zertifikate/"
LOGPATH="/var/log/certbot/"
LOGFILE="renew.log"

if [ ! -e $LOGPATH ]
then
	mkdir "$LOGPATH"
	touch "$LOGPATH$LOGFILE"
fi

EXPIRYDATE=`echo "QUIT" | openssl s_client -connect $HOST:$SSLPORT 2>/dev/null | openssl x509 -noout -enddate 2>/dev/null|sed 's/notAfter=//g'`
#echo $EXPIRYDATE

EXPIRYDATE_epoch=$(date --date "$EXPIRYDATE" +%s)

CURRENT_DATE_epoch=`date +%s`

epochDiff=`echo "$EXPIRYDATE_epoch" - "$CURRENT_DATE_epoch"|bc`

### Get difference of days
dayDiff=`echo "$epochDiff"/86400|bc`

if [ "$dayDiff" -le "$RENEWDAY" ] ; then
#	/etc/init.d/emby-server stop
#	sleep 2
#	/etc/init.d/nginx stop 
#	sleep 2
#	/etc/init.d/fw stop
#	sleep 2
	$CMDRENEW > $LOGPATH$LOGFILE 2>&1 
	openssl pkcs12 -inkey "$CMDLETSENCRYPT"live/"$HOST"/privkey.pem -in "$CMDLETSENCRYPT"live/"$HOST"/fullchain.pem -export -out "$CMDSSLDEST""$HOST".pfx -passout pass:
#	/etc/init.d/fw start
#	sleep 2
#	/etc/init.d/nginx start
#	sleep 2
#	/etc/init.d/emby-server start
	
else
	echo "There is "$dayDiff" days left for the certificate of "$HOST" and the autorenew is allowed for "$RENEWDAY" days or less" > "$LOGPATH$LOGFILE" 2>&1
fi
```

Weiterhin sollte ein Crontab zum täglichen durchführen eingerichtet werden.

1. `crontab -e`
2. Folgendes ganz unten einfügen und speichern `0 3 * * * /home/Scriptordner/Scriptname.sh` um täglich um 3 Uhr in der Früh das Script auszuführen.

#### Quelle:

https://github.com/shrom59/letsencryptemby/blob/master/renewvod