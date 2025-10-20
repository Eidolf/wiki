---
title: webserverzertifikat-ausstellen-mit-windows-ca-und-openssl
description: 
published: true
date: 2023-12-31T13:37:22.137Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:37:18.791Z
---

# Webserverzertifikat ausstellen mit Windows CA und OpenSSL

1\. Anforderung über [http://WindowsCA/certsrv](http://windowsca/certsrv) ausstellen und speichern als "Webserver.req"

2\. Haken bei Anforderung speichern herausnehmen und das Zertifikat lokal installieren.

3\. Zertifikat mit privaten Schlüssel exportieren als "Webserver.pfx"

4\. Das PKCS12 Zertifikat "Webserver.pfx" in eine PEM Datei mit OpenSSL konvertieren

```
openssl pkcs12 -in Webserver.pfx -out Webserver.pem -nodes
```

5\. Die "Webserver.pem" Datei mit einem Texteditor öffnen und die Hashwerte des Keys und des Zertifikats kopieren und jeweils eine "Webserver.key" und "Webserver.cer" damit anlegen.