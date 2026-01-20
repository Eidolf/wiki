---
title: E-Mail
description: 
published: true
date: 2026-01-20T16:27:20.506Z
tags: spf, dkim, dmarc
editor: markdown
dateCreated: 2023-12-31T13:27:07.358Z
---

# SPF (Sender Policy Framework)

Das Sender Policy Framework ist ein Verfahren, mit dem das Fälschen der Absenderadresse einer E-Mail verhindert werden soll, genauer das Versenden von E-Mail über nicht legitimierte Mail Transfer Agents unterbindet. Es entstand als Verfahren zur Abwehr von Spam

## Einrichtung

Im DNS muss auf Domänenebene ein TXT Eintrag gesetzt werden z.B. mit folgendem Wert.  
`v=spf1 mx ip4:99.99.99.99 -all`  
In diesem Beispiel wird:
1. Die Version **SPF1** verwendet
2. Alle **MX** Einträge dürfen senden
3. Zusätzlich darf der Server mit der IP **99.99.99.99** senden
4. Durch **-all** sind alle anderen IP´s unberechtigt unter diesem Domänennamen zu senden.

Für ein schnelles erstellen bzw. mehr Informationen dazu kann man auch einen Generator verwenden.  
[SPF Generator](https://mxtoolbox.com/SPFRecordGenerator.aspx)

### Quelle:

https://www.spf-record.de/syntax

# DKIM (DomainKeys Identified Mail)

Aus dem Englischen übersetzt-DomainKeys Identified Mail ist eine E-Mail-Authentifizierungsmethode, mit der gefälschte Absenderadressen in E-Mails erkannt werden. Diese Methode wird häufig bei Phishing- und E-Mail-Spam eingesetzt.

## Einrichtung

1. Erstellen von einem privaten und einem öffentlichen Schlüssel mit openSSL
2. Der Freitext ist der DKIM Selektor, dieser ist wichtig für den Abgleich und sollte wenn möglich eindeutig gewählt werden (Beispiele hierführ **mail**,**email** oder ganz frei **mail-dkim-domain**).
`openssl genrsa -out freitext.private.key 1024`
`openssl rsa -in freitext.private.key -pubout -out freitext.public.key`
3. Den privaten Schlüssel an seinem ausgehenden E-Mail Server hinterlegen oder einpflegen.
4. DNS Eintrag für die Domäne erstellen mit folgenden Werten 
4.1. Name = `freitext._domainkey`
4.2. Typ = `TXT`
4.3. Wert = `v=DKIM1; k=rsa; p=Wert-des-Public-Key-ohne-Begin/End-Public-Key`

### Quelle:

https://www.checkdomain.de/support/nameserver-dns/nutzung-einrichtung-dns/dkim-domainkey-schluessel-erstellen-und-nutzen/

# DMARC (Domain-based Message Authentication, Reporting and Conformance)

Domain-based Message Authentication, Reporting and Conformance ist eine Spezifikation, die entwickelt wurde, um Missbrauch von E-Mails zu reduzieren

## Einrichten

1. DNS Eintrag erstellen mit folgenden Werten 
1.1. Name = `_dmarc`
1.2. Typ = `TXT`
1.3. Wert = `v=DMARC1; p=none; rua=e-mail@adresse.de; ruf=e-mail@adresse.de; fo=1`
2. Die Werte im Beispiel entsprechen folgenden Einstellungen 
2.1. **v=DMARC1** = Version von DMARC
2.2. **p=none** = Anweisung, wie mit Mails der Hauptdomäne zu verfahren ist, bei **none** soll der Empfänger die Mails direkt bekommen.
2.3. **rua=E-Mail** = Aggregierter Report wird versandt an:
2.4. **ruf=E-Mail** = Forensischer Report wird versandt an:

Für ein schnelles erstellen bzw. mehr Informationen dazu kann man auch einen Generator verwenden.  
[DMARC Generator](https://mxtoolbox.com/DMARCRecordGenerator.aspx)

Zusätzlich kann es sein das der Empfänger von den Reports in einer anderen Domäne ist.  
In folgendem Beispiel wollen wir für **subdomain.de** den Bericht an **postmaster@hauptdomain.de** senden.

1. Es muss in der Hauptdomain folgender DNS Eintrag gesetzt werden 
1.1. Name = `subdomain.de._report._dmarc`
1.2. Typ = `TXT`
1.3. Wert = `v=DMARC1`

### Quelle:
https://www.msxfaq.de/spam/dmarc.htm
https://de.wikipedia.org/wiki/DMARC

