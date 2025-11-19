---
title: AD Funktionsebene / Functional Level
description: 
published: true
date: 2025-11-19T13:55:44.968Z
tags: domain, ad, active directory, forest
editor: markdown
dateCreated: 2025-11-19T12:54:56.687Z
---

# Anheben der Funktionsebene
## Vorbereitung
1. Alle DCs prüfen
Jeder Domain Controller muss mindestens die Zielversion des Functional Levels unterstützen.
Beispiel: Für Windows Server 2016 Functional Level müssen alle DCs mindestens Windows Server 2016 sein.
2. Gesundheitscheck der AD-Umgebung
`dcdiag /v` → Diagnose aller DCs.
`repadmin /replsummary` → Replikationsstatus prüfen.
`dcdiag /test:dns /v` → DNS-Integrität sicherstellen.
3. Backup erstellen
System State Backup aller DCs.
Optional: VM-Snapshots für schnelle Wiederherstellung.
4. Aktuelle Functional Levels prüfen
```
Get-ADForest | Select ForestMode
Get-ADDomain | Select DomainMode
```

## Upgrade-Schritte
Domain Funktionsebene anheben >
[Anheben des Domain Function Levels](/de/Wiki-Seiten/Microsoft/Server/Rollen/AD/ad-anheben-des-domain-function-levels)

Forest Funktionsebene anheben >
[Anheben des Forest Function Levels](/de/Wiki-Seiten/Microsoft/Server/Rollen/AD/ad-anheben-des-forest-function-levels)