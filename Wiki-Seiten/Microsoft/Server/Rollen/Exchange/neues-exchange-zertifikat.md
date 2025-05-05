---
title: neues-exchange-zertifikat
description: 
published: true
date: 2025-05-05T13:26:08.079Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:33:36.155Z
---

# Neues Exchange Zertifikat

# Erstellen:

Befehl für ein neuen Request der an eine Zertifizierungsstelle eingereicht wird (alles in eine Zeile):

```
New-ExchangeCertificate -generaterequest `
-subjectname "dc=com,dc=contoso,c=DE,o=Contoso-Corporation,cn=exchange.contoso.com" `
-IncludeAcceptedDomains `
-IncludeAutoDiscover `
-privatekeyexportable $true `
-domainname CAS01,CAS01.exchange.corp.constoso.com,exchange.contoso.com, autodiscover.contoso.com `
-path c:\certrequest_cas01.txt
```

# Importieren:

`
Import-ExchangeCertificate -path c:\<certificate_file_name>.cer -friendlyname "Contoso CAS01"
`

den **frienlyname** muss man nicht angeben

# Quelle:

http://msexchangefaq.de/howto/e2k7ssl.htm
http://technet.microsoft.com/en-us/library/aa995942.aspx