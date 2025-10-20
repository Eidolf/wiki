---
title: geplante-aufgaben
description: 
published: true
date: 2024-05-06T08:24:54.260Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:28:39.995Z
---

# Geplante Aufgaben

# Powershell Script ausführen

1. Dem Dienstkonto an den auszuführenden Ordnern und/oder Dateien Berechtigungen erteilen.
2. Unter Aktionen folgendes eingeben 
	2.1. Programm/Skript = `PowerShell`
	2.2. Argumente = `-command "C:\Skriptpfad\Skript.ps1"`
	2.3. Starten in = Pfad wo Dienstkonto zugriff hat

# Neustart einstellen

1. Ausführen als lokaler Admin
2. Unter Aktionen folgendes eingeben 
	2.1. Programm/Skript = shutdown.exe aus system32 auswählen
	2.2. Argumente = `-r -t 30`