---
title: letsencrypt
description: 
published: true
date: 2024-04-04T15:18:38.395Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:37:08.917Z
---

# Letsencrypt

## Certbot Windows Installation

Die modernste und einfachste Art ist in aktuellen Windows Versionen eine Installation mit winget

1. PowerShell öffnen
2. `winget install certbot`

## Zertifikat mit CSR erstellen

Eigentlich ist es nicht vorgesehen einen CSR an Let's Encrypt einzureichen, doch für Testzwecke manchmal ein gangbarer Weg.

1. CSR erstellen und speichern
2. PowerShell öffnen
3. Bei Windows in den Ordner von Certbot wechseln, Standardpfad: `C:\Program Files\Certbot\bin`
4. Anfrage einreichen mit folgendem Befehl `./certbot certonly --manual --csr /path/to/file.csr --preferred-challenges dns`
5. Im Wizard wird durch das Flag **"--preferred-challenges dns"** am Ende gefordert einen DNS TXT Eintrag auf **"\_acme-challenge.&lt;YOUR\_DOMAIN&gt;"** zur Autorisierung zu setzen, somit sollte man Zugriff auf einen öffentlichen DNS haben.
6. Falls alles korrekt durchgelaufen ist, werden die Zertifikate im "Certbot\\bin" Ordner abgelegt. Beschreibung zu den einzelnen Dateien ist am Ende im Wizard sichtbar.

## Zertifikate erneuern

Der Vorgang ist relativ einfach, da eigentlich nur der Befehl `certbot` verwendet wird.  
Doch gibt es folgendes zu beachten falls ein Reverse Proxy verwendet wird und **certbot** / **letsencrypt** somit auf Port 443 das falsche Zertifikat angezeigt bekommt.  
Der Befehl um eine Abfrage über Port 80 zu erzwingen ist folgender:  
`certbot --preferred-challenges http`

### Quelle:

https://certbot.eff.org/docs/using.html

## SAN Zertifikate erstellen

`certbot -d wiki.eidolf.de -d www.eidolf.de --cert-name Zertifikatsnamen`

### Quelle:

https://community.letsencrypt.org/t/adding-san-to-a-certificate/103954/2

## PowerShell

### Quelle:

https://mssec.wordpress.com/2019/10/25/using-powershell-to-get-wildcard-certificate-from-lets-encrypt/