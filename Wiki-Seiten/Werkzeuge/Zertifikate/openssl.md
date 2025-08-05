---
title: openssl
description: 
published: true
date: 2025-08-05T14:20:15.871Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:37:13.727Z
---

# OpenSSL

## Standard Aufgaben

### X509 CSR generieren

`c:\\OpenSSL-Win64\\bin\\openssl.exe req -config c:\\OpenSSL-Win64\\bin\\openssl.cfg -new -newkey rsa:2048 -nodes -keyout domaenenname.de.key -out domaenenname.de.csr`

### PKCS12 Export

`c:\\OpenSSL-Win64\\bin\\openssl.exe pkcs12 -config c:\\OpenSSL-Win64\\bin\\openssl.cfg -export -out certificate(PKCS12).pfx -inkey domaenenname.de.key -in domaenenname.de.crt -certfile CACert(Intermediate).crt`

### Zertifikate einer Kette anzeigen

`c:\\OpenSSL-Win64\\bin\\openssl.exe storeutl -noout -text -certs Zertifikatskette.crt`

## Erweiterte Aufgaben

### SAN Zertifikat generieren

1.  openssl.cfg Datei kopieren und in openssl-san.cfg umbenennen.  
    Falls keine Config Datei vorhanden ist habe ich hier grob eine zusammengestellt inkl. einem SAN Eintrag.

	Für Code bitte ausklappen: >>>
<details>
  <summary>Ausklappen</summary>
  Folgende Änderungen in der .cfg vornehmen

```
[req] req_extensions = v3_req

[ v3_req ]

# Extensions to add to a certificate request

basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = server1.yourdomain.tld
DNS.2 = mail.yourdomain.tld
DNS.3 = www.yourdomain.tld
DNS.4 = www.sub.yourdomain.tld
DNS.5 = mx.yourdomain.tld
DNS.6 = support.yourdomain.tld
```
</details>
  
2.  Schlüssel generieren
2.1 ohne Passwort
`openssl genrsa -out srvr1-yourdomain-tld-2048.key 2048`
2.2. mit Passwort
`openssl genrsa -aes256 -passout pass:Passwort -out srvr1-yourdomain-tld-2048-with-PW.key 2048`

3.  Request erstellen

`openssl req -new -out srvr1-yourdomain-tld-2048.csr -key srvr1-yourdomain-tld-2048.key -config openssl-san.cfg`

4.  Gegentest ob die Anforderung stimmt

`openssl req -text -noout -in san_domain_com.csr`

#### Quelle:

http://apetec.com/support/GenerateSAN-CSR.htm
http://wiki.cacert.org/FAQ/subjectAltName

### SAN Zertifikat generieren LINUX

1.  san.cnf erstellen
	1.1 Inhalt 
  ```
  [ req ]
  default_bits       = 4096
  distinguished_name = req_distinguished_name
  req_extensions     = req_ext
  [ req_distinguished_name ]
  countryName                 = Country Name (2 letter code)
  stateOrProvinceName         = State or Province Name (full name)
  localityName               = Locality Name (eg, city)
  organizationName           = Organization Name (eg, company)
  commonName                 = Common Name (e.g. server FQDN or YOUR name)
  [ req_ext ]
  subjectAltName = @alt_names
  [alt_names]
  DNS.1   = FQDN 1
  DNS.2   = FQDN 2
  DNS.3   = usw. FQDN
  ```

2.  CSR und Key Datei erstellen
	2.1 Befehl ausführen
  `openssl req -out sslcert.csr -newkey rsa:2048 -nodes -keyout private.key -config san.cnf`

3.  Eventuell noch in eine PEM Datei umwandeln
	3.1 Befehl ausführen
  `openssl x509 -in sslcert.csr -outform PEM -out sslcert.pem`

#### Quelle:

https://geekflare.com/san-ssl-certificate/

### Keyfile aus PFX/P12 Datei extrahieren

Der erste Befehl exportiert nur das Keyfile aus der PFX Datei, man muss hierbei ein PEM Passphrase eingeben das länger als 4 Zeichen ist  
`openssl pkcs12 -in certname.pfx -nocerts -out certname.key -password pass:"Passwort"`  
Falls der Zielserver kein PEM Passphrase verträgt kann man dieses mit folgendem Befehl entfernen  
`openssl rsa -in private.key -out NewKeyFile.key -passin pass:PEMPassphrase`

#### Quelle:

https://www.ibm.com/support/knowledgecenter/en/SSVP8U_9.7.0/com.ibm.drlive.doc/topics/r_extratsslcert.html  
https://serverfault.com/questions/515833/how-to-remove-private-key-password-from-pkcs12-container

### Privaten Schlüssel mit Zertifikat und Request vergleichen

Im Falle es gibt ein Zertifikat bei dem man nicht mehr weiß ob der private Schlüssel passend dazu ist kann man mit folgenden Befehlen einen Vergleich ausführen.  
Alle ausgegebenen MD5 Hash Werte müssen übereinstimmen.

-   Zertifikat: `openssl x509 –noout –modulus –in Zertifikat.crt | openssl md5`
-   Schlüsseldatei: `openssl rsa –noout –modulus –in Schlüsseldatei.key | openssl md5`
-   Anforderungsdatei: `openssl req -noout –modulus –in Anforderungsdatei.csr | openssl md5`

#### Quelle:

https://www.ssl247.de/kb/ssl-certificates/troubleshooting/certificate-matches-private-key

### Privaten Schlüssel mit Passwort überprüfen

Ab und an hat man eine Schlüssel Datei liegen und weiß nicht ob ein Passwort verwendet wurde oder nicht.
Dies kann mit einem Texteditor deiner Wahl ganz einfach über den Header herausgefunden werden.
Wenn hier **Encrypted** enthalten ist, wurde ein Passwort gesetzt.
![openssl-001.png](/media/openssl-001.png)

Um zu prüfen ob das Passwort mit der Datei übereinstimmt kann man folgenden Befehl verwenden

`openssl rsa -in srvr1-yourdomain-tld-2048-with-PW.key`

![openssl-002.png](/media/openssl-002.png)

## Fehler

### Fehler bei erstellen eines v3 Zertifikats auf Windows Systemen

**WARNING: can’t open config file: /usr/local/ssl/openssl.cnf**  
Hier wird der falsche Pfad zu einer .cnf Datei gesucht. Lösung:

1.  CMD Fenster öffnen
2.  `set OPENSSL_CONF=c:\[PATH TO YOUR OPENSSL DIRECTORY]\bin\openssl.cfg`

#### Quelle:

http://jaspreetchahal.org/warning-cant-open-config-file-usrlocalsslopenssl-cnf/

## Download Link Windows

[OpenSSL Windows](http://slproweb.com/products/Win32OpenSSL.html)

## Quellen für Befehle

http://shib.kuleuven.be/docs/ssl_commands.shtml 
https://www.sslshopper.com/article-most-common-openssl-commands.html

## Linux

### Umwandeln cer in pem (DER zu x509)

`openssl x509 -inform DER -in certificate.cer -out certificate.pem`  
Wird für Linux Apache Server benötigt um von Windows CA ausgestellte Zertifikate lesbar zu machen.