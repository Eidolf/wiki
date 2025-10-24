---
title: Exchange 2013
description: 
published: true
date: 2025-10-24T15:43:51.023Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:32:49.402Z
---

# ECP

## Maximale Anzahl angezeigter Organisationseinheiten bei neuer Mailbox

Da ab 2013 die ECP im Web angezeigt wird gibt es eine Web Config Einstellung die Organisationseinheiten (OU) auf standardmäßig 500 angezeigte Objekte reduziert.  
Es ist möglich den Wert zu erweitern um größere Umgebungen über die ECP anzuzeigen. Die Einstellung erfolgt pro Server.

### Lösung:

1. Genau Anzahl abrufen: `(Get-OrganizationalUnit -ResultSize unlimited).count`
2. Exchange Programmpfad zu ECP Ordner öffnen z.B.: `C:\Program Files\Microsoft\Exchange Server\V15\ClientAccess\ecp`
3. Die **web.config** öffnen und den Wert aus **Punkt 4** überhalb von **&lt;/appSettings&gt;** einfügen
4. `<!-- allows the OU picker when placing a new mailbox in its designated organizational unit to retrieve all OUs - default value is 500 -->`
5. Neustart des IIS Anwendungspools **MSExchangeECPAppPool**

### Quelle:

https://support.microsoft.com/de-de/help/3038717/exchange-server-doesn-t-display-all-ous-when-it-creates-a-new-mailbox

# Outlook Web App

## S/MIME

Ab Exchange 2013 SP1 (CU4) gibt es wieder die Möglichkeit Mails zu signieren und zu verschlüsseln. Die Erstkonfiguration läuft über Exchange Powershell Befehle:

1. Set-SmimeConfig -OWAAllowUserChoiceOfSigningCertificate $true -OWACRLRetrievalTimeout 10000 -OWAEncryptionAlgorithms 6602:128
2. Get-ChildItem –Path "Cert:\\LocalMachine\\CA\\RootZert-Thumprint/Fingerabdruck"| Export-Certificate -Type SST -FilePath C:\\Zertifikate\\Root.CA.sst
3. Set-SmimeConfig -SMIMECertificateIssuingCA (Get-Content C:\\Zertifikate\\Root.CA.sst -Encoding Byte)
4. In OWA eine Mail öffnen und unter den erweiterten Nachrichtenoptionen S/MIME aktivieren
5. S/MIME MSI installieren, wird nach dem ersten aktivieren aufgerufen

### Quelle:

https://technet.microsoft.com/de-de/library/dn554259(v=exchg.150).aspx