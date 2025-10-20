---
title: exchange-adressbuch-synchronisieren
description: 
published: true
date: 2023-12-31T13:33:02.171Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:32:59.009Z
---

# Exchange Adressbuch synchronisieren

Das Adressbuch sychronisiert sich jedes mal beim anmelden mit seinem Profil und Intervallmäßig zwischen 1min und 60min um eine gute Netzwerkverteilung zu haben. Da der Server Standarmäßig sein Adressbuch mit dem AD um 1:30Uhr Nachts synchronisiert werden den ganzen Tag die gleichen Daten gezogen.

# <span class="mw-headline" id="bkmrk-fehler">Fehler</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Adressbuch_synchronisieren&action=edit&section=1 "Abschnitt bearbeiten: Fehler")<span class="mw-editsection-bracket">\]</span></span>

Wenn man feststellt das, daß Communicator Adressbuch sich nicht aktualisiert muss man den Ordner

```
C:\Users\Benutzername\AppData\Local\Microsoft\Communicator\sip_Benutzername@domäne.de
```

löschen. Danach werden alle Daten neu vom Server gezogen und die aktualisierungen sollten wieder funktionieren.

Für eine Zeiteinstellung der Sychronisierung kann man den Registry den Wert " GalDownloadInitialDelay "( DWORD ) unter

<div class="vector-body" id="bkmrk-32bit-hkey_current_u"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- 32Bit <dl><dd>**HKEY\_CURRENT\_USER\\Software\\Policies\\Microsoft\\Communicator**</dd></dl>
- 64Bit <dl><dd>**HKEY\_CURRENT\_USER\\Software\\Wow6432Node\\Policies\\Microsoft\\Communicator**</dd></dl>

</div></div></div>setzen und die Dezimalwerte 0 - 60 eingeben für die Minutenanzahl.

Quellen:

```
<a class="external free" href="http://www.tincupsandstring.com/2009/12/01/forcing-address-book-download/" rel="nofollow">http://www.tincupsandstring.com/2009/12/01/forcing-address-book-download/</a> 
<a class="external free" href="http://www.shudnow.net/2010/01/20/forcing-address-book-updates-in-communicator-2007-r2/" rel="nofollow">http://www.shudnow.net/2010/01/20/forcing-address-book-updates-in-communicator-2007-r2/</a>
<a class="external free" href="http://social.technet.microsoft.com/Forums/en/ocsplanningdeployment/thread/0765020d-86b6-48bd-a9d5-7a753b249f59" rel="nofollow">http://social.technet.microsoft.com/Forums/en/ocsplanningdeployment/thread/0765020d-86b6-48bd-a9d5-7a753b249f59</a>
```