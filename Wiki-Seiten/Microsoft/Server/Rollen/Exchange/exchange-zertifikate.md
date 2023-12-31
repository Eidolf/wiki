# Exchange Zertifikate

# <span class="mw-headline" id="bkmrk-exchange-zertifikate-1">Exchange Zertifikate ausstellen</span>

[Exchange Zertifikat erneuern](https://wiki.eidolf.de/index.php/Exchange_Zertifikat_erneuern "Exchange Zertifikat erneuern")  
[Neues Exchange Zertifikat](https://wiki.eidolf.de/index.php/Neues_Exchange_Zertifikat "Neues Exchange Zertifikat")

# <span class="mw-headline" id="bkmrk-exchange-zertifikat--2">Exchange Zertifikat einen Dienst zuordnen</span>

Um einen Exchange Zertifikat eine bestimmten Dienst zuzuordnen z.B. IIS für OWA und ActiveSync, dann folgenden Befehl eigeben.

```
Enable-ExchangeCertificate -Thumbprint <String> -Services "None, IMAP, POP, UM, IIS, SMTP"
```

Quelle:

```
<a class="external free" href="http://technet.microsoft.com/de-de/library/aa997231.aspx" rel="nofollow">http://technet.microsoft.com/de-de/library/aa997231.aspx</a>
```

# <span class="mw-headline" id="bkmrk-exchange-zertifikat--4">Exchange Zertifikat mit mehreren beinhalteten Hostnamen</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_Zertifikate&action=edit&section=3 "Abschnitt bearbeiten: Exchange Zertifikat mit mehreren beinhalteten Hostnamen")<span class="mw-editsection-bracket">\]</span></span>

Quelle:

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/aa995942.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/aa995942.aspx</a>
```