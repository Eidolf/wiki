# Exchange Edge Fehler

# <span class="mw-headline" id="bkmrk-nachrichten-werden-n-1">Nachrichten werden nicht mehr Empfangen</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Edge_Fehler&action=edit&section=1 "Abschnitt bearbeiten: Nachrichten werden nicht mehr Empfangen")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-speicherplatz-proble-1">Speicherplatz Probleme</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Edge_Fehler&action=edit&section=2 "Abschnitt bearbeiten: Speicherplatz Probleme")<span class="mw-editsection-bracket">\]</span></span>

Es ist nicht genügend Speicherplatz, &lt;=4GB kann Probleme verursachen.

## <span class="mw-headline" id="bkmrk-zertifikatsprobleme">Zertifikatsprobleme</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Edge_Fehler&action=edit&section=3 "Abschnitt bearbeiten: Zertifikatsprobleme")<span class="mw-editsection-bracket">\]</span></span>

Das Exchange Zertifikat wurde geändert oder ist abgelaufen und der Edge Server muss dem Exchange erst wieder bekannt gemacht werden.  
Voraussetzung hierfür, das das Exchange Zertifikat erneuert wurde --&gt; [Exchange Zertifikat erneuern](https://wiki.eidolf.de/index.php/Exchange_Zertifikat_erneuern "Exchange Zertifikat erneuern")  
An der Edge Server Powershell

```
New-EdgeSubscription -Filename "c:\subscription.xml"
```

  
Danach wieder auf den Exchange Server

<div class="vector-body" id="bkmrk-die-erstellte-%22subsc"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Die erstellte "subscription.xml" lokal kopieren
2. Die Exchange Management Console öffnen und eine neue Edge subscription einrichten. Die bestehende nicht löschen, um alle Sende/Empfangs Konnektoren intakt zu lassen.

</div></div></div>*Organization Configuration -&gt; Hub Transport -&gt; New Edge Subscription...* --&gt; subscription.xml auswählen und "Automatically create a Send Connector" auswählen.

<div class="vector-body" id="bkmrk-wenn-eine-fehlermeld"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Wenn eine Fehlermeldung erscheint das die Uhrzeit nicht synchron ist, dann die Uhren zwischen Exchange und Edge Server anpassen
2. Danach erscheint eine Warnung das ein gewisser Port freigeschalten gehört

</div></div></div>In der Edge Server Powershell eine Synchronisierung anstoßen

```
Start-EdgeSynchronization
```

  
Danach die Warteschlange abarbeiten lassen, evtl. anstoßen  
  
Quelle:

```
<a class="external free" href="http://www.bunkerhollow.com/blogs/matt/archive/2009/06/11/exchange-2007-edge-queue-4-7-5-certificate-validation-failure.aspx" rel="nofollow">http://www.bunkerhollow.com/blogs/matt/archive/2009/06/11/exchange-2007-edge-queue-4-7-5-certificate-validation-failure.aspx</a>
```