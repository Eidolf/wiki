---
title: emby
description: 
published: true
date: 2023-12-31T13:36:47.577Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:36:42.903Z
---

# Emby

# <span class="mw-headline" id="bkmrk-zertifikate-1">Zertifikate</span>

## <span class="mw-headline" id="bkmrk-letsencrypt-1">LetsEncrypt</span>

### <span class="mw-headline" id="bkmrk-erstes-zertifikat-au-1">Erstes Zertifikat ausstellen</span>

Dieses Beispiel wird an einem Ubuntu 16.04 Server durchgeführt.

<div class="vector-body" id="bkmrk-installation-von-cer"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Installation von Certbot siehe [Certbot Website](https://certbot.eff.org/#ubuntuxenial-other)
2. Port 80 an Emby Server freischalten bzw. über Firewall weiterleiten
3. `letsencrypt certonly --standalone -d beispiel.domain.de`
4. Wechseln zum Ordner in dem das Zertifikat ausgestellt wurde **/etc/letsencrypt/live/beispiel.domain.de/**
5. In dem Ordner gibt es 4 Dateien **cert.pem** **chain.pem** **fullchain.pem** **privkey.pem**, diese **NICHT** löschen da diese zum erneuern benötigt werden.
6. `openssl pkcs12 -inkey privkey.pem -in fullchain.pem -export -out /var/lib/emby-server/ssl/beispiel.domain.de.pfx -passout pass:`
7. In der Emby Server Verwaltung das erstellte Zertifikat auswählen.
8. Emby Server neu starten.

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://emby.media/community/index.php?/topic/42315-creating-a-letsencrypt-ssl-certificate-for-emby/" rel="nofollow">https://emby.media/community/index.php?/topic/42315-creating-a-letsencrypt-ssl-certificate-for-emby/</a>
```

### <span class="mw-headline" id="bkmrk-automatisch-update-s-1">Automatisch Update Script</span>

Da das Script in der Quelle nicht mehr zu finden ist poste ich es hier komplett rein, es ist nicht mein Werk und falls der Urheber das Posten anmerkt werde ich es komplett löschen.

<div class="vector-body" id="bkmrk-installation-von-bc%C2%A0"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Installation von bc `apt-get install bc`
2. Script

</div></div></div>```
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

<div class="vector-body" id="bkmrk-crontab--e-folgendes"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `crontab -e`
2. Folgendes ganz unten einfügen und speichern `0 3 * * * /home/Scriptordner/Scriptname.sh` um täglich um 3 Uhr in der Früh das Script auszuführen.

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://github.com/shrom59/letsencryptemby/blob/master/renewvod" rel="nofollow">https://github.com/shrom59/letsencryptemby/blob/master/renewvod</a>
```