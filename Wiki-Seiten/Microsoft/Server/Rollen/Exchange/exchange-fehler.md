# Exchange Fehler

# <span class="mw-headline" id="bkmrk-exchange-allgemein-1">Exchange allgemein</span>

## <span class="mw-headline" id="bkmrk-webservices-funktion-1">Webservices funktionieren nicht mehr</span>

Bei dem Zugriff auf eine OWA Seite oder auf die ActiveSync Seite wird im Browser ein " Fehler 500 " angezeigt. Dies kann mehrer Ursachen haben.

<div class="vector-body" id="bkmrk-unter-dem-punkt-%28-an"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Unter dem Punkt ( Anwendungspool → Rechtklick → Anwendungspoolstandardwerte Einstellen ) ist eventuell der 32Bit Modus aktiviert worden

</div></div></div>  
Quelle:

```
<a class="external free" href="http://www.administrator.de/index.php?content=139514" rel="nofollow">http://www.administrator.de/index.php?content=139514</a>
```

# <span class="mw-headline" id="bkmrk-exchange-2010-1">Exchange 2010</span>

## <span class="mw-headline" id="bkmrk-mmc-1">MMC</span>

<div class="vector-body" id="bkmrk-fehler%3A-beim-verbind"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Fehler: ```
    Beim Verbinden mit dem Remoteserver ist folgender Fehler aufgetreten: Die Anforderung kann vom
     WS-Verwaltungsdienst nicht verarbeitet werden. Das Systemlastenkontingent von 1000 Anforderungen pro 2 Sekunden wurde
    überschritten. Senden Sie künftige Anforderungen mit einer geringeren Übertragungsrate, oder erhöhen Sie das Systemkont
    ingent. Die nächste Anforderung dieses Benutzers wird mindestens -2147483648 Millisekunden nicht genehmigt. Weitere Inf
    ormationen finden Sie im Hilfethema "about_Remote_Troubleshooting".
        + CategoryInfo          : OpenError: (System.Manageme....RemoteRunspace:RemoteRunspace) [], PSRemotingTransportExc
       eption
        + FullyQualifiedErrorId : PSSessionOpenFailed
    ```
    
    Lösung:  
    Neustart des Servers ausführen.

</div></div></div>## <span class="mw-headline" id="bkmrk-zustellungsfehler-1">Zustellungsfehler</span>

Fehler: Mails von Extern und von Online Web Access werden einwandfrei an die Interne Zieladresse versendet. Intern über Outlook erscheint aber folgdende Fehlermeldung.

```
550 5.1.1 RESOLVER.ADR.ExRecipNotFound; not found 
```

Lösung:  
Auf Festplatte C nach " **\*.nk2** " suchen und diese umbenennen, hierdurch wird der Namenscache von Outlook geleert. Es werden keine alten Adressen mehr mit der Empfangsadresse verknüpft.  
Ausführlichere Beschreibung in der Quellenangabe zu finden.

Quelle:

```
<a class="external free" href="http://fawzi.wordpress.com/2008/04/11/exchange-2007-delivery-has-failed-to-these-recipients-or-distribution-lists/" rel="nofollow">http://fawzi.wordpress.com/2008/04/11/exchange-2007-delivery-has-failed-to-these-recipients-or-distribution-lists/</a>
```