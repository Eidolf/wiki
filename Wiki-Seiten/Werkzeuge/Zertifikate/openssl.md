# OpenSSL

## <span class="mw-headline" id="bkmrk-standard-aufgaben-1">Standard Aufgaben</span>

### <span class="mw-headline" id="bkmrk-x509-csr-generieren-1">X509 CSR generieren</span>

c:\\OpenSSL-Win64\\bin\\openssl.exe req -config c:\\OpenSSL-Win64\\bin\\openssl.cfg -new -newkey rsa:2048 -nodes -keyout domaenenname.de.key -out domaenenname.de.csr

### <span class="mw-headline" id="bkmrk-pkcs12-export-1">PKCS12 Export</span>

c:\\OpenSSL-Win64\\bin\\openssl.exe pkcs12 -config c:\\OpenSSL-Win64\\bin\\openssl.cfg -export -out certificate(PKCS12).pfx -inkey domaenenname.de.key -in domaenenname.de.crt -certfile CACert(Intermediate).crt

### <span class="mw-headline" id="bkmrk-zertifikate-einer-ke-1">Zertifikate einer Kette anzeigen</span>

c:\\OpenSSL-Win64\\bin\\openssl.exe storeutl -noout -text -certs Zertifikatskette.crt

## <span class="mw-headline" id="bkmrk-erweiterte-aufgaben-1">Erweiterte Aufgaben</span>

### <span class="mw-headline" id="bkmrk-san-zertifikat-gener-1">SAN Zertifikat generieren</span>

1\. openssl.cfg Datei kopieren und in openssl-san.cfg umbenennen.  
Falls keine Config Datei vorhanden ist habe ich hier grob eine zusammengestellt inkl. einem SAN Eintrag.

<div class="mw-collapsible mw-collapsed mw-made-collapsible" id="bkmrk-ausklappen"><span aria-expanded="false" class="mw-collapsible-toggle mw-collapsible-toggle-default mw-collapsible-toggle-collapsed" role="button" tabindex="0"><a class="mw-collapsible-text">Ausklappen</a></span></div>Für Code bitte ausklappen: &gt;&gt;&gt;

2\. Folgende Änderungen in der .cfg vornehmen

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

3\. Schlüssel generieren

```
openssl genrsa -out srvr1-yourdomain-tld-2048.key 2048
```

4\. Request erstellen

```
openssl req -new -out srvr1-yourdomain-tld-2048.csr -key srvr1-yourdomain-tld-2048.key -config openssl-san.cfg
```

5\. Gegentest ob die Anforderung stimmt

```
openssl req -text -noout -in san_domain_com.csr
```

#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://apetec.com/support/GenerateSAN-CSR.htm" rel="nofollow">http://apetec.com/support/GenerateSAN-CSR.htm</a>
<a class="external free" href="http://wiki.cacert.org/FAQ/subjectAltName" rel="nofollow">http://wiki.cacert.org/FAQ/subjectAltName</a>
```

### <span class="mw-headline" id="bkmrk-san-zertifikat-gener-3">SAN Zertifikat generieren LINUX</span>

1\. san.cnf erstellen

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

2\. CSR und Key Datei erstellen

2.1 Befehl ausführen

```
openssl req -out sslcert.csr -newkey rsa:2048 -nodes -keyout private.key -config san.cnf
```

3\. Eventuell noch in eine PEM Datei umwandeln

3.1 Befehl ausführen

```
openssl x509 -in sslcert.csr -outform PEM -out sslcert.pem
```

#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://geekflare.com/san-ssl-certificate/" rel="nofollow">https://geekflare.com/san-ssl-certificate/</a>
```

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-keyfile-aus-pfx%2Fp12--1">Keyfile aus PFX/P12 Datei extrahieren</span>

Der erste Befehl exportiert nur das Keyfile aus der PFX Datei, man muss hierbei ein PEM Passphrase eingeben das länger als 4 Zeichen ist  
`openssl pkcs12 -in certname.pfx -nocerts -out certname.key -password pass:"Passwort"`  
Falls der Zielserver kein PEM Passphrase verträgt kann man dieses mit folgendem Befehl entfernen  
`openssl rsa -in private.key -out NewKeyFile.key -passin pass:PEMPassphrase`

#### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://www.ibm.com/support/knowledgecenter/en/SSVP8U_9.7.0/com.ibm.drlive.doc/topics/r_extratsslcert.html" rel="nofollow">https://www.ibm.com/support/knowledgecenter/en/SSVP8U_9.7.0/com.ibm.drlive.doc/topics/r_extratsslcert.html</a>
<a class="external free" href="https://serverfault.com/questions/515833/how-to-remove-private-key-password-from-pkcs12-container" rel="nofollow">https://serverfault.com/questions/515833/how-to-remove-private-key-password-from-pkcs12-container</a>
```

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-privaten-schl%C3%BCssel-m-1">Privaten Schlüssel mit Zertifikat und Request vergleichen</span>

Im Falle es gibt ein Zertifikat bei dem man nicht mehr weiß ob der private Schlüssel passend dazu ist kann man mit folgenden Befehlen einen Vergleich ausführen.  
Alle ausgegebenen MD5 Hash Werte müssen übereinstimmen.

- Zertifikat: `openssl x509 –noout –modulus –in Zertifikat.crt | openssl md5`
- Schlüsseldatei: `openssl rsa –noout –modulus –in Schlüsseldatei.key | openssl md5`
- Anforderungsdatei: `openssl req -noout –modulus –in Anforderungsdatei.csr | openssl md5`

#### <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="https://www.ssl247.de/kb/ssl-certificates/troubleshooting/certificate-matches-private-key" rel="nofollow">https://www.ssl247.de/kb/ssl-certificates/troubleshooting/certificate-matches-private-key</a>
```

## <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

### <span class="mw-headline" id="bkmrk-fehler-bei-erstellen-1">Fehler bei erstellen eines v3 Zertifikats auf Windows Systemen</span>

**WARNING: can’t open config file: /usr/local/ssl/openssl.cnf** Hier wird der falsche Pfad zu einer .cnf Datei gesucht. Lösung:

1. CMD Fenster öffnen
2. ```
    set OPENSSL_CONF=c:\[PATH TO YOUR OPENSSL DIRECTORY]\bin\openssl.cfg
    ```

#### <span class="mw-headline" id="bkmrk-quelle%3A-9">Quelle:</span>

```
<a class="external free" href="http://jaspreetchahal.org/warning-cant-open-config-file-usrlocalsslopenssl-cnf/" rel="nofollow">http://jaspreetchahal.org/warning-cant-open-config-file-usrlocalsslopenssl-cnf/</a>
```

## <span class="mw-headline" id="bkmrk-download-link-window-1">Download Link Windows</span>

[OpenSSL Windows](http://slproweb.com/products/Win32OpenSSL.html)

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-quellen-f%C3%BCr-befehle-1">Quellen für Befehle</span>

```
<a class="external free" href="http://shib.kuleuven.be/docs/ssl_commands.shtml" rel="nofollow">http://shib.kuleuven.be/docs/ssl_commands.shtml</a>
<a class="external free" href="https://www.sslshopper.com/article-most-common-openssl-commands.html" rel="nofollow">https://www.sslshopper.com/article-most-common-openssl-commands.html</a>
```

## <span class="mw-headline" id="bkmrk-linux-1">Linux</span>

### <span id="bkmrk--3"></span><span class="mw-headline" id="bkmrk-umwandeln-cer-in-pem-1">Umwandeln cer in pem (DER zu x509)</span>

`openssl x509 -inform DER -in certificate.cer -out certificate.pem`  
Wird für Linux Apache Server benötigt um von Windows CA ausgestellte Zertifikate lesbar zu machen.