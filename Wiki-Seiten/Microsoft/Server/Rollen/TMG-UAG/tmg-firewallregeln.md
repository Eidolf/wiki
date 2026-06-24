---
title: tmg-firewallregeln
description: 
published: true
date: 2026-06-24T16:16:44.004Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:36:30.243Z
---

# TMG Firewallregeln

# WebDAV mit SSL in DMZ

1. Publishing Regel erstellen mit folgenden Einstellungen
2. Reiter Nach: Zielserver angeben mit IP Adresse, Ursprünglichen Hostheader weiterleiten anhaken, Ursprung ist der ursprüngliche Client
3. Linkübersetzung: Linkübersetzung für diese Regel anwenden
4. Authentifizierungsdeligierung: Keine Delegierung aber direkte Authentifiezierung des Clients
5. Öffentlicher Name: Hier den Namen angeben auf dem von außen zugegriffen wird.
6. Pfade: wie intern und das Virtuelle Verzeichnis angeben z.B. " /VirtuellesVerzeichnis "
7. Bridging: Anforderung an SSL-Port umleiten mit Standardport (443)
8. Benutzer: Alle Benutzer hinzufügen
9. Listner:
	9.1. Netzwerke: Entweder Extern oder eine direkte IP angeben
  9.2. Verbindungen: SSL Verbindung 443
  9.3. Zertifikate: Jeder IP ein Zertifikat zuweisen (Ausgestelltes Serverzertifikat für die Externe Adresse)
  9.4. Authentifizierung: Keine Authentifizierung