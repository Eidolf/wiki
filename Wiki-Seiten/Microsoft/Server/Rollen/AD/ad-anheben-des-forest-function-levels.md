---
title: AD Anheben des Forest Function Levels
description: 
published: true
date: 2025-11-19T16:52:19.750Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:08.652Z
---

# Beschreibung
Das Forest und Domain Level sollte bestmöglich auf dem höchsten Stand gehalten werden um alle Funktionen nutzen zu können.

# Durchführung

1. Active Directory Domains and Trusts (Domänen und Vertrauenstellung) öffnen
![forest-001.png](/media/forest-001.png)
2. Auf dem obersten Punkt mit einem Rechtsklick > Raise Forest Function Level  
![forest-002.png](/media/forest-002.png)
3. Version auswählen und bestätigen  
![forest-003.png](/media/forest-003.png)
4. Eine Warnung bestätigen und hoffentlich folgendes Fenster sehen
![forest-007.png](/media/forest-007.png)

## Wer es ohne GUI machen möchte
`Set-ADForestMode -Identity <ForestName> -ForestMode Windows2016Forest`

# Nacharbeiten
Auch hier kann man entweder die Automatische Synchronisierung abwarten oder manuelle Domainsynchronisierung durchführen.

1. Prüfen des Forest Level mit PowerShell `Get-ADForest | Select ForestMode`
2. Synchronisierung anstoßen > `repadmin /syncall /AdeP`
3. Prüfen der Replizierung > `repadmin /showrepl`
4. Wie bei den Domänen eine Allgemeine Prüfung durchführen
