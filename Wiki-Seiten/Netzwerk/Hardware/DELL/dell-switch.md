---
title: Dell-Switch
description: 
published: true
date: 2025-07-24T15:28:10.003Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:36:01.166Z
---

# VLAN

Untenstehende Einstellungen werden für das Beispiel [Hyper-V VLAN](/de/Wiki-Seiten/Microsoft/Server/Rollen/Hyper-V/hyper-v#vlan) verwendet.  
Weitere Informationen zu VLANs sind hier zu finden > [VLAN](/de/Wiki-Seiten/Netzwerk/vlan).

## Neues VLAN anlegen

1. Unter dem Reiter VLAN Membership 
![dell-vlan-001.png](/media/dell-vlan-001.png)
2. Auf Hinzufügen klicken 
![dell-vlan-002.png](/media/dell-vlan-002.png)
3. VLAN ID und Name eingeben
![dell-vlan-003.png](/media/dell-vlan-003.png)

## An Port binden

1. Unter dem Reiter VLAN Port Settings, einen Port auswählen.
2. VLAN Einstellungen setzen 
![dell-vlan-004.png](/media/dell-vlan-004.png)

## An LAG binden

1. Unter dem Reiter VLAN LAG Settings, eine LAG Gruppe auswählen.
2. VLAN Einstellungen setzen 
![dell-vlan-005.png](/media/dell-vlan-005.png)

## Port oder LAG Taggen

1. Unter dem Reiter VLAN Membership 
![dell-vlan-001.png](/media/dell-vlan-001.png)
2. Die VLAN ID auswählen und die LAG hinzufügen (Tagged = T) 
![dell-vlan-006.png](/media/dell-vlan-006.png)