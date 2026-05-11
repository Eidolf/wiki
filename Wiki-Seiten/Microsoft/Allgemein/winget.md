---
title: winget
description: Ein Paketmanager auf Windows bei dem man über Befehlzeile Software installieren kann
published: true
date: 2026-05-11T16:28:39.358Z
tags: package manager, install, powershell
editor: markdown
dateCreated: 2026-05-11T16:28:39.358Z
---

# Winget auf Server 2025 aktivieren
Eigentlich ist auf Server 2025 winget vorinstalliert, falls es trotzdem nicht vorhanden sein sollte kann man es auch nachinstallieren.

Es gibt zwei Ansätze, entweder den richtigen Client, der sollte über den Microsoft Store installiert werden bzw. kann man auch etwas umständlich das appx Paket organisieren und das installieren oder die einfach Art das PowerShell Modul.
Beim PowerShell Modul sind die befehle aber anders aufgebaut als mit dem normalen winget.

## Vorgehen
1. PowerShell Administrativ öffnen
2. `Install-Module -Name Microsoft.WinGet.Client -Force`
3. Prüfen ob alles sauber installiert ist mit `Get-Command *winget*`