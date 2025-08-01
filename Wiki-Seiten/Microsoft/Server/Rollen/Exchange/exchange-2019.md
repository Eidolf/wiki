---
title: Exchange 2019
description: Alles rund um den Exchange Server 2019
published: true
date: 2025-08-01T15:41:36.235Z
tags: microsoft, exchange, office365, e-mail
editor: markdown
dateCreated: 2025-07-18T16:21:56.178Z
---

# Installation
> Auch bei dieser Anleitung zur Exchange 2019 Installation ist es inkl. Upgrade von einer Exchange 2016 Umgebung und somit etwas gemischt vom Aufbau.
> Zum Zeitpunkt an dem sie geschrieben wird ist sie eigentlich veraltet, da der Support auch für 2019 im Oktober 2025 endet.
> Eigentlich entsteht die Anleitung nur, da es eine Vorbereitung für das Upgrade auf Exchange SE ist, den ich privat aber nicht mehr verwenden werde.
> Ich gehe zwar auf die korrekten Voraussetzungen ein, werde aber schlussendlich keine Empfohlenen Werte nehmen. Stellt einfach viel Ressourcen für den Exchange bereit, Microsoft Produkte werden wieder extrem hungrig.
{.is-info}

## Voraussetzungen

### Festplatten
Es sollten getrennte Festplatten für folgende Bereiche existieren
- Betriebssystem (C:) 80GB - NTFS
- Exchange Anwendung (E:) 130GB - NTFS
- Exchange Datenbanken und Datenbank Logs (F:) 130GB - ReFS 
- Exchange Anwendung / Web Logs (G:) 30GB - ReFS
- Exchange Queue (H:) 20GB - ReFS
> Dies ist in meinen Augen die mindest Aufteilung um eine einigermaßen gute Geschwindigkeit zu bekommen.
> Für einen Firmeneinsatz würde ich aber noch weiter aufteilen z.B. pro Datenbank eine Platte und dazugehörige Logs ausgliedern.
> Weiterhin die Logs zwischen Anwendung und Web-Diensten trennen.
> Ausführlich bei Microsoft zu finden https://learn.microsoft.com/de-de/exchange/plan-and-deploy/deployment-ref/storage-configuration
{.is-warning}

![ex2019-install-001.png](/media/ex2019-install-001.png)

### Software und Rollen
Da ich den Exchange auf einem Server 2025 installieren werde, sollten die Voraussetzungen schon gegeben sein.
Microsoft schlägt selbst vor eine Server Core Installation für den Exchange zu verwenden, mir ist es aber zu heikel bzw. zu umständlich im Fehlerfall nicht alle Tools direkt auf dem Server zu haben.

**.Net Framework 4.8** ist standardmäßig installiert und unterstützt.
<sub>siehe MS Unterstützungsmatrix > https://learn.microsoft.com/de-de/exchange/plan-and-deploy/supportability-matrix#net-framework</sub>

Die nötigen Rollen werden mit einem Haken im Installations Wizard mitinstalliert.

### Active Directory

Das Active Directory muss typischerweise auf einen neuen Exchange Server vorbereitet werden, zu dem komme ich im nächsten Punkt.
Meine vorhandene AD 2022 Umgebung ist unterstützt.
<sub> siehe MS Unterstützungsmatrix > https://learn.microsoft.com/de-de/exchange/plan-and-deploy/supportability-matrix#supported-active-directory-environments </sub>

> Eine Gesamtstruktur unter 2012 R2 wird nicht unterstützt und muss erstmal auf aktuellen Stand gebracht werden.
{.is-warning}

## AD Vorbereitung
Exchange Installationen benötigen eine AD Schema Erweiterung, diese habe ich unter Exchange 2016 > Update > Punkt 2 beschrieben.
Ich verlinke es hier, da das gleiche Vorgehen ist.
[Exchange-2016#update](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-2016#update)