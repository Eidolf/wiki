# Neues Exchange Zertifikat

# <span class="mw-headline" id="bkmrk-erstellen%3A-1">Erstellen:</span>

Befehl für ein neuen Request der an eine Zertifizierungsstelle eingereicht wird (alles in eine Zeile):

```
New-ExchangeCertificate
-generaterequest
-subjectname "dc=com,dc=contoso,c=DE,o=Contoso-Corporation,cn=exchange.contoso.com"
-IncludeAcceptedDomains
-IncludeAutoDiscover
-privatekeyexportable $true
-domainname CAS01,CAS01.exchange.corp.constoso.com,exchange.contoso.com, autodiscover.contoso.com
-path c:\certrequest_cas01.txt
```

# <span class="mw-headline" id="bkmrk-importieren%3A-1">Importieren:</span>

```
Import-ExchangeCertificate -path c:\<certificate_file_name>.cer -friendlyname "Contoso CAS01"
```

den **frienlyname** muss man nicht angeben

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://msexchangefaq.de/howto/e2k7ssl.htm" rel="nofollow">http://msexchangefaq.de/howto/e2k7ssl.htm</a>
<a class="external free" href="http://technet.microsoft.com/en-us/library/aa995942.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/aa995942.aspx</a>
```