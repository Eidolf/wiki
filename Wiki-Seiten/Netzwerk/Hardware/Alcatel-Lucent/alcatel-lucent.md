---
title: Alcatel Lucent
description: 
published: true
date: 2025-07-24T15:17:17.124Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:35:56.378Z
---

# Konsolen Kabel

Alcatel verwendet eine eigene Belegung für ihr RJ45 zu DB9 Kabel, folgend die Belegung 
![alcatel-6xxx-console.png](/media/alcatel-6xxx-console.png)

# Authentifizierung

## Standard Anmeldedaten

Name: admin  
Passwort: switch

## HTTP freischalten

`aaa authentication http local`

# IP Interface verwalten

## Anzeigen

`show ip interface`

## Erstellen

`ip interface "Interface Name" vlan 1 address 192.168.0.254 mask 255.255.255.0`

## Löschen

`no ip interface "Interface Name"`

# Port Mirroring verwalten

## Anzeigen

`show port mirroring status`

## Erstellen

`port mirroring 1 source 1/2 destination 1/5`

## Löschen

`no port mirroring 1`

# Konfiguration Speicher

## Anzeigen welcher Modus benutzt wird

Es gibt Certified (Schreibgeschützter Modus) und Working  
`show running-directory`

## Speichermöglichkeiten

### Running > Working

`write memory`

### Working > Certified

`copy working certified [flash-synchro]`  
**flash-synchro** synchronisiert die Konfiguration auf alle Steckplätze

### Running > Working, im Certified Mode

`configuration snapshot all <file>`  
Die Datei muss in **working/boot.cfg** verschoben werden.

## Konfiguration ansehen

`show configuration snapshot [all|vlan|ip|...]`

## Von Certified in Working Mode wechseln

`reload working no rollback-timeout`

# Switch zurücksetzen

Quelle:
http://www.alcatelunleashed.com/viewtopic.php?t=23244