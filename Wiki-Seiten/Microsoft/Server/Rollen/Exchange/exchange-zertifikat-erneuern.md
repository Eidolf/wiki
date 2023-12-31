# Exchange Zertifikat erneuern

# <span class="mw-headline" id="bkmrk-exchange-2007-1">Exchange 2007</span>

Am Exchange 2007 funktioniert die Zertifikatsverwaltung nur über die Exchange Shell, hier muss man folgenden Befehl eingeben um das Standardmäßige Exchange Zertifikat zu erneuern.

```
Get-ExchangeCertificate | fl
Get-ExchangeCertificate -thumbprint “Hashwert SMTP zu finden im vorherigen Befehl” | New-ExchangeCertificate
```

Bei dem zweiten Befehl kommt eine Anfrage ob man das Standardmäßige vorhande Zertifikat mit dem neuen (gleich Hashwert kopieren) ersetzen will, dies mit "JA" beantworten.  
Jetzt das neue Zertifikat überprüfen ob es richtig eingebunden wurde.

```
Get-ExchangeCertificate -thumbprint “neuer Hashwert” | fl
```

Wenn alles richtig funktioniert kann man das alte Zertifikat löschen

```
Remove-ExchangeCertificate -Thumbprint “alter Hashwert”
```

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://exchangepedia.com/2008/01/exchange-server-2007-renewing-the-self-signed-certificate.html" rel="nofollow">http://exchangepedia.com/2008/01/exchange-server-2007-renewing-the-self-signed-certificate.html</a>
```

# <span class="mw-headline" id="bkmrk-exchange-2010-1">Exchange 2010</span>

Wenn man am Exchange 2010 über die Verwaltungskonsole eine Zertifikat verlängern will, kommt eine .req Datei heraus die beim einreichen an die Zertifizierungsstelle nicht gelesen werden kann. Am einfachsten ist es diese Datei an eine online Konvertierungsstelle einzureichen um eine **Base64** Datei herauszubekommen.

<div class="vector-body" id="bkmrk-go-thru-the-emc-and-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Go thru the EMC and create the .req file, which will be binary by default.
2. Navigate to the following site [http://www.motobit.com/util/base64-decoder-encoder.asp](http://www.motobit.com/util/base64-decoder-encoder.asp). Click the choose file button, upload your .req file, and click the convert to source data button to get the output.
3. Once you see the output, paste the contents in your favorite text editor
4. Sandwich the text above with certificate header and footer. Both should be on separate lines. So make sure the file starts off with this text:  
    -----BEGIN CERTIFICATE-----  
    Hashwert einfügen aus der Base64 Datei  
    -----END CERTIFICATE-----
5. Den gesamten Wert kopieren und als erweiterte Zertifikatsanforderung bei einer Zertifizierungstelle einreichen.
6. Am Ende die Zertifikatserneuerung abschließen mit der frisch erstellten .cer Datei

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/exchangesvrsecuremessaging/thread/f570e4bd-7194-4cf5-92f4-c7ada2f5dc8a" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/exchangesvrsecuremessaging/thread/f570e4bd-7194-4cf5-92f4-c7ada2f5dc8a</a>
```