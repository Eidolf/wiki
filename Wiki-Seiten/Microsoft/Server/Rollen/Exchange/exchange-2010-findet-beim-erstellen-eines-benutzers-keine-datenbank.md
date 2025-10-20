---
title: exchange-2010-findet-beim-erstellen-eines-benutzers-keine-datenbank
description: 
published: true
date: 2023-12-31T14:32:52.638Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:32:49.417Z
---

# Exchange 2010 Findet beim erstellen eines Benutzers keine Datenbank

# <span class="mw-headline" id="bkmrk-fehlermeldung%3A">Fehlermeldung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2010_Findet_beim_erstellen_eines_Benutzers_keine_Datenbank&action=edit&section=1 "Abschnitt bearbeiten: Fehlermeldung:")<span class="mw-editsection-bracket">\]</span></span>

```
Error:
Load balancing failed to find a valid mailbox database.
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2010_Findet_beim_erstellen_eines_Benutzers_keine_Datenbank&action=edit&section=2 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Die Datenbank wurde von der Bereitstellung (Provisioning)ausgenommen und sollte wieder automatisch Verfügbar gemacht werden

## <span class="mw-headline" id="bkmrk-befehl-zum-testen%3A">Befehl zum testen:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2010_Findet_beim_erstellen_eines_Benutzers_keine_Datenbank&action=edit&section=3 "Abschnitt bearbeiten: Befehl zum testen:")<span class="mw-editsection-bracket">\]</span></span>

```
Get-MailboxDatabase | fl isexcludedfromprovisioning,issuspendedfromprovisioning
```

Eventuell bei mehreren Datenbanken, die zu suchende angeben

```
Get-MailboxDatabase -identity "Datenbankname" | fl isexcludedfromprovisioning,issuspendedfromprovisioning 
```

## <span class="mw-headline" id="bkmrk-befehl-zum-aktiviere-1">Befehl zum aktivieren:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2010_Findet_beim_erstellen_eines_Benutzers_keine_Datenbank&action=edit&section=4 "Abschnitt bearbeiten: Befehl zum aktivieren:")<span class="mw-editsection-bracket">\]</span></span>

```
Set-MailboxDatabase <database name> -IsExcludedFromProvisioning $False
```

# <span class="mw-headline" id="bkmrk-andere-ursachen%3A">Andere Ursachen:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2010_Findet_beim_erstellen_eines_Benutzers_keine_Datenbank&action=edit&section=5 "Abschnitt bearbeiten: Andere Ursachen:")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-mailboxdatenbank-ist"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Mailboxdatenbank ist nicht eingebunden
2. Exchange Mailboxrolle ist nicht installiert

</div></div></div># <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2010_Findet_beim_erstellen_eines_Benutzers_keine_Datenbank&action=edit&section=6 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://www.johnmen.com/index.php?id=55" rel="nofollow">http://www.johnmen.com/index.php?id=55</a>
```