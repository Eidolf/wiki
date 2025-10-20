---
title: exchange-neue-verteilergruppen-authentifizieren
description: 
published: true
date: 2023-12-31T13:33:25.308Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:33:22.292Z
---

# Exchange Neue Verteilergruppen Authentifizieren

Unter Exchange 2007 und 2010 sind neu angelegte Verteilerlisten standardmäßig von extern nicht erreichbar, weil die Option “Authentifizierung aller Absender anfordern” in den Einstellungen “Einschränkungen für die Nachrichtenübermittlung” aktiviert ist. Dabei ist es unerheblich, ob die Verteilergruppe grafisch über die Exchange Verwaltungskonsole oder mit dem Powershell Befehl new-DistributionGroup angelegt wurde.

Eine einzelne Verteilerliste wird mit folgendem Befehl für den Empfang von E-Mails von außerhalb der Organisation mit folgendem Befehl aktiviert:

```
set-DistributionGroup -identity "NeueVerteilergruppe" -RequireSenderAuthenticationEnabled $false
```

  
Will man alle bestehenden Verteilerlisten in einem Befehl bearbeiten, geht das selbstverständlich auch:

```
Get-DistributionGroup | set-DistributionGroup -RequireSenderAuthenticationEnabled $false
```

<div class="vector-body" id="bkmrk--2"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">---

</div></div></div>Quelle:

```
<a class="external free" href="http://www.roland-ehle.de/archives/589" rel="nofollow">http://www.roland-ehle.de/archives/589</a>
```