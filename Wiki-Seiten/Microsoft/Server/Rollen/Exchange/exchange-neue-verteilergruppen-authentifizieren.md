---
title: Exchange neue Verteilergruppen authentifizieren
description: 
published: true
date: 2026-08-26T12:53:38.892Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:33:22.292Z
---

# Beschreibung

Unter Exchange 2007 und 2010 sind neu angelegte Verteilerlisten standardmäßig von extern nicht erreichbar, weil die Option “Authentifizierung aller Absender anfordern” in den Einstellungen “Einschränkungen für die Nachrichtenübermittlung” aktiviert ist. Dabei ist es unerheblich, ob die Verteilergruppe grafisch über die Exchange Verwaltungskonsole oder mit dem Powershell Befehl new-DistributionGroup angelegt wurde.

Eine einzelne Verteilerliste wird mit folgendem Befehl für den Empfang von E-Mails von außerhalb der Organisation mit folgendem Befehl aktiviert:

```powershell
set-DistributionGroup -identity "NeueVerteilergruppe" -RequireSenderAuthenticationEnabled $false
```

  
Will man alle bestehenden Verteilerlisten in einem Befehl bearbeiten, geht das selbstverständlich auch:

```powershell
Get-DistributionGroup | set-DistributionGroup -RequireSenderAuthenticationEnabled $false
```

# Quelle:
http://www.roland-ehle.de/archives/589