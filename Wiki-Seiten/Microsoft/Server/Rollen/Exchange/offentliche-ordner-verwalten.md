---
title: offentliche-ordner-verwalten
description: 
published: true
date: 2023-12-31T14:34:10.691Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:07.485Z
---

# Öffentliche Ordner verwalten

# <span class="mw-headline" id="bkmrk-ordner-anlegen-1">Ordner anlegen</span>

Das Ordner anlegen funktioniert über die Exchange Verwaltungskonsole unter Tools --&gt; Öffentliche Ordner verwalten

# <span class="mw-headline" id="bkmrk-ordner-berechtigunge-1">Ordner Berechtigungen</span>

Die Berechtigungen sind bei Exchange 2007/10 nur über die Exchange Shell für die einzelnen Ordner nachträglich zu setzen.

<div class="vector-body" id="bkmrk-abfrage-und-ausgabe-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Abfrage und Ausgabe der Berechtigungen eines bestimmten öffentlichen Ordners in eine Textdatei

</div></div></div>```
Get-PublicFolderClientPermission -Identity "\öffentlicherOrdnername\BeispielUnterordner" |fl > c:\PFAccess.txt
```

<div class="vector-body" id="bkmrk-hinzuf%C3%BCgen-von-einem"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Hinzufügen von einem Benutzer mit Editor Rechten

</div></div></div>```
add-PublicFolderClientPermission -Identity "\öffentlicherOrdnername\BeispielUnterordner" -AccessRights Editor -user Benutzername
```

Zugriffsrechte sind folgende: Owner | Editor | Reviewer

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/bb310789.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/bb310789.aspx</a>
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-verwalten-%C3%BCber-shell-1">Verwalten über Shell</span>

## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://www.msxfaq.de/howto/e2k7pfmanagement.htm" rel="nofollow">http://www.msxfaq.de/howto/e2k7pfmanagement.htm</a>
```