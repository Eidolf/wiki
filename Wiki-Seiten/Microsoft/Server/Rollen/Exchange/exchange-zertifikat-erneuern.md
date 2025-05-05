---
title: exchange-zertifikat-erneuern
description: 
published: true
date: 2025-05-05T13:16:59.966Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:58.000Z
---

# Exchange Zertifikat erneuern

## Exchange 2007

Am Exchange 2007 funktioniert die Zertifikatsverwaltung nur über die Exchange Shell, hier muss man folgenden Befehl eingeben um das Standardmäßige Exchange Zertifikat zu erneuern.

```
Get-ExchangeCertificate | fl
Get-ExchangeCertificate -thumbprint “Hashwert SMTP zu finden im vorherigen Befehl” | New-ExchangeCertificate
```

Bei dem zweiten Befehl kommt eine Anfrage ob man das Standardmäßige vorhande Zertifikat mit dem neuen (gleich Hashwert kopieren) ersetzen will, dies mit "JA" beantworten.  
Jetzt das neue Zertifikat überprüfen ob es richtig eingebunden wurde.

`Get-ExchangeCertificate -thumbprint “neuer Hashwert” | fl`

Wenn alles richtig funktioniert kann man das alte Zertifikat löschen

`Remove-ExchangeCertificate -Thumbprint “alter Hashwert”`

### Quelle:

http://exchangepedia.com/2008/01/exchange-server-2007-renewing-the-self-signed-certificate.html

## Exchange 2010

Wenn man am Exchange 2010 über die Verwaltungskonsole eine Zertifikat verlängern will, kommt eine .req Datei heraus die beim einreichen an die Zertifizierungsstelle nicht gelesen werden kann. Am einfachsten ist es diese Datei an eine online Konvertierungsstelle einzureichen um eine **Base64** Datei herauszubekommen.

1. Go thru the EMC and create the .req file, which will be binary by default.
2. Navigate to the following site [http://www.motobit.com/util/base64-decoder-encoder.asp](http://www.motobit.com/util/base64-decoder-encoder.asp). Click the choose file button, upload your .req file, and click the convert to source data button to get the output.
3. Once you see the output, paste the contents in your favorite text editor
4. Sandwich the text above with certificate header and footer. Both should be on separate lines. So make sure the file starts off with this text:  
    -----BEGIN CERTIFICATE-----  
    Hashwert einfügen aus der Base64 Datei  
    -----END CERTIFICATE-----
5. Den gesamten Wert kopieren und als erweiterte Zertifikatsanforderung bei einer Zertifizierungstelle einreichen.
6. Am Ende die Zertifikatserneuerung abschließen mit der frisch erstellten .cer Datei

### Quelle:

http://social.technet.microsoft.com/Forums/en-US/exchangesvrsecuremessaging/thread/f570e4bd-7194-4cf5-92f4-c7ada2f5dc8a

## Exchange 2016 > CU23 und 2019 > CU12

> Untenstehende Anleitung ist für Zertifikate die nicht vom Exchange selbst ausgestellt wurden, für ein z.B. self signed SMTP Zertifikat kann man immer noch die Anleitung von Exchange 2007 befolgen.
{.is-info}

Ab den obengenannten Versionen wurde aus den Exchange Servern die Möglichkeit entfernt die Zertifikate über die EAC zu erneuern.

1. Als erstes muss das aktuell verwendete Zertifikat herausgefunden werden. 
`Get-ExchangeCertificate | Where {$_.IsSelfSigned -eq $false} | Format-List FriendlyName, CertificateDomains, Thumbprint, NotAfter`
2. Den Fingerabdruck des zu erneuernden Zertifikates kopieren und im nächsten Befehl einfügen
3. Den Fingerabdruck hier einfügen um die Variable zu befüllen
`$certrequest = Get-ExchangeCertificate -Thumbprint 'Fingerabdruck' | New-ExchangeCertificate -GenerateRequest -PrivateKeyExportable:$true`
4. Request Datei mit folgendem Befehl erstellen, hier unter dem c:\temp Verzeichnis
`[System.IO.File]::WriteAllBytes('\\EX-NetBiosName\C$\temp\mailserver.beispieldomaene.de.csr', [System.Text.Encoding]::Unicode.GetBytes($certrequest))`
5. Den Request an der Zertifizierungsstelle einreichen
6. Das ausgestellte Zertifikat entweder über PowerShell am Exchange importieren oder auch direkt über die Zertifikate MMC
`Import-ExchangeCertificate -FriendlyName mailserver.beispieldomaene.de -FileData ([System.IO.File]::ReadAllBytes('\\EX-NetBiosName\C$\temp\mailserver.beispieldomaene.de.cer')) -PrivateKeyExportable $true`
7. Abschließend kann man die Dienste weiterhin über die EAC zuweisen oder wie verlinkt über die PowerShell
[Exchange-Zertifikat-einen-Dienst-zuordnen](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-zertifikate#exchange-zertifikat-einen-dienst-zuordnen)

### Quelle:
https://supertekboy.com/2023/07/08/renew-a-certificate-in-exchange-2016-2019/