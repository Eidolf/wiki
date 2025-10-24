---
title: Exchange Fehler
description: 
published: true
date: 2025-10-24T15:38:23.135Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:33.862Z
---

# Exchange allgemein

## Webservices funktionieren nicht mehr

Bei dem Zugriff auf eine OWA Seite oder auf die ActiveSync Seite wird im Browser ein " Fehler 500 " angezeigt. Dies kann mehrer Ursachen haben.

- Unter dem Punkt ( Anwendungspool → Rechtklick → Anwendungspoolstandardwerte Einstellen ) ist eventuell der 32Bit Modus aktiviert worden

Quelle:
http://www.administrator.de/index.php?content=139514

# Exchange 2010

## MMC

1. Fehler: 
```
Beim Verbinden mit dem Remoteserver ist folgender Fehler aufgetreten: Die Anforderung kann vom
WS-Verwaltungsdienst nicht verarbeitet werden. Das Systemlastenkontingent von 1000 Anforderungen pro 2 Sekunden wurde
überschritten.
Senden Sie künftige Anforderungen mit einer geringeren Übertragungsrate,
oder erhöhen Sie das Systemkontingent. Die nächste Anforderung dieses Benutzers wird mindestens
-2147483648 Millisekunden nicht genehmigt. Weitere Informationen finden Sie im Hilfethema "about_Remote_Troubleshooting".
   + CategoryInfo          : OpenError: (System.Manageme....RemoteRunspace:RemoteRunspace) [], PSRemotingTransportException
   + FullyQualifiedErrorId : PSSessionOpenFailed
``` 
    
Lösung:  
Neustart des Servers ausführen.

## Zustellungsfehler

Fehler: Mails von Extern und von Online Web Access werden einwandfrei an die Interne Zieladresse versendet. Intern über Outlook erscheint aber folgdende Fehlermeldung.

`
550 5.1.1 RESOLVER.ADR.ExRecipNotFound; not found 
`

Lösung:  
Auf Festplatte C nach " **\*.nk2** " suchen und diese umbenennen, hierdurch wird der Namenscache von Outlook geleert. Es werden keine alten Adressen mehr mit der Empfangsadresse verknüpft.  
Ausführlichere Beschreibung in der Quellenangabe zu finden.

Quelle:

http://fawzi.wordpress.com/2008/04/11/exchange-2007-delivery-has-failed-to-these-recipients-or-distribution-lists/
