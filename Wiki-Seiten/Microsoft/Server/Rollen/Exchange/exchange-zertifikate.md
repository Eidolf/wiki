---
title: exchange-zertifikate
description: 
published: true
date: 2024-06-03T09:49:28.429Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:02.899Z
---

# Exchange Zertifikate

# Exchange Zertifikate ausstellen
[Exchange-Zertifikate-Erneuern](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-zertifikate)
[Neues-Exchange-Zertifikat](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/neues-exchange-zertifikat)

# Exchange Zertifikat einen Dienst zuordnen

Um einen Exchange Zertifikat eine bestimmten Dienst zuzuordnen z.B. IIS für OWA und ActiveSync, dann folgenden Befehl eigeben.

`Enable-ExchangeCertificate -Thumbprint <String> -Services "None, IMAP, POP, UM, IIS, SMTP"`

Quelle:

http://technet.microsoft.com/de-de/library/aa997231.aspx

# Exchange Zertifikat mit mehreren beinhalteten Hostnamen

## Quelle:

http://technet.microsoft.com/en-us/library/aa995942.aspx