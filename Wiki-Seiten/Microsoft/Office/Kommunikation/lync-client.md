# Lync Client

# <span class="mw-headline" id="bkmrk-webkonferenz-1">Webkonferenz</span>

Wenn mal kein Lync Client zur Hand ist, kann man entweder selbst den Webdienst von Lync nutzen oder einem Bekannten die Webadresse zur Verfügung stellen um eine Konferenz mit ihm zu starten.

## <span class="mw-headline" id="bkmrk-sofortkonferenz-1">Sofortkonferenz</span>

Die Sofortkonferenz erstellt einen Link, den man händisch einem Konferenzpartner zur Verfügung stellen muss oder man selbstständig die Konferenzpartner im Lync hinzufügt. Ich beschränke die Anleitung auf eine Webeinladung, da einen Benutzer in eine Lync Konferenz hinzuzufügen selbst erklärend ist.

### <span class="mw-headline" id="bkmrk-sofortkonferenz-erst-1">Sofortkonferenz erstellen:</span>

[![LyncClient01.jpg](https://wiki.eidolf.de/images/b/b8/LyncClient01.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient01.jpg)

[![LyncClient02.jpg](https://wiki.eidolf.de/images/9/9c/LyncClient02.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient02.jpg)

Ein neues Fenster öffnet sich, somit ist die Sofortkonferenz erstellt.

### <span class="mw-headline" id="bkmrk-link-abrufen%3A-1">Link abrufen:</span>

[![LyncClient03.jpg](https://wiki.eidolf.de/images/b/bf/LyncClient03.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient03.jpg)

[![LyncClient04.jpg](https://wiki.eidolf.de/images/4/4f/LyncClient04.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient04.jpg)

[![LyncClient05.jpg](https://wiki.eidolf.de/images/c/c6/LyncClient05.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient05.jpg)

Das gelb markierte ist der Link der dem gegenüber zugeschickt werden muss.

### <span class="mw-headline" id="bkmrk-gegenseite-ruft-den--1">Gegenseite ruft den Link auf:</span>

Wenn die Gegenseite einen Lync Client installiert hat, dann macht dieser die Konferenz auf. Wenn kein Client installiert ist wird versucht die Webseite zu öffnen. Die Webseite bringt eine Zertifikatswarnung bei allen Benutzern die nicht von mir das Root Zertifikat meiner Domäne haben. Einfach eine Sicherheitsausnahme bestätigen.

[![LyncClient06.jpg](https://wiki.eidolf.de/images/thumb/b/b3/LyncClient06.jpg/500px-LyncClient06.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient06.jpg)

Danach können zwei arten von Fenstern erscheinen, bei beiden jeweils "erzwingen" das man mit dem Browser teilnehmen will.

[![LyncClient07.jpg](https://wiki.eidolf.de/images/thumb/8/8a/LyncClient07.jpg/500px-LyncClient07.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient07.jpg)

[![LyncClient08.jpg](https://wiki.eidolf.de/images/thumb/1/13/LyncClient08.jpg/500px-LyncClient08.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient08.jpg)

### <span class="mw-headline" id="bkmrk-plugin-installieren%3A-1">Plugin installieren:</span>

Nachdem der Link für den Gast aufgerufen wird, erscheint nochmal eine Sicherheitswarnung, da sie Seite auf eine andere wechselt um ein Plugin zu installieren. Dieses Plugin zulassen und installieren.

[![LyncClient09.jpg](https://wiki.eidolf.de/images/thumb/6/6f/LyncClient09.jpg/500px-LyncClient09.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient09.jpg)

### <span class="mw-headline" id="bkmrk-gast-anmeldung-1">Gast Anmeldung</span>

Der Gast gibt einfach irgendeinen Alias an und bestätigt dies mit "An Besprechung Teilnehmen"

[![LyncClient10.jpg](https://wiki.eidolf.de/images/thumb/9/9e/LyncClient10.jpg/500px-LyncClient10.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient10.jpg)

Danach wird der Gast verbunden und er sieht folgendes Fenster (das Chatfenster wurde extra aufgemacht)

[![LyncClient11.jpg](https://wiki.eidolf.de/images/thumb/8/84/LyncClient11.jpg/500px-LyncClient11.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient11.jpg)

Von Lync Seite schaut es folgendermaßen aus

[![LyncClient12.jpg](https://wiki.eidolf.de/images/thumb/b/b6/LyncClient12.jpg/500px-LyncClient12.jpg)](https://wiki.eidolf.de/index.php/Datei:LyncClient12.jpg)

Man kann nun mit dem Gast Schreiben, Audiokommunikation durchführen und Präsentieren.

## <span id="bkmrk--12"></span><span class="mw-headline" id="bkmrk-erzwungener-beitritt-1">Erzwungener Beitritt über Webbrowser</span>

Falls die URL z.B. folgende ist **https://meet.contoso.com/Vorname.Nachname/6H89DRE**  
Dann kann man mit **?=sl1** erzwingen über web beizutreten auch wenn S4B/Lync installiert ist  
Die URL würde folgendermaßen aussehen **https://meet.contoso.com/Vorname.Nachname/6H89DRE?=sl1**