---
title: tmg-firewallregeln
description: 
published: true
date: 2023-12-31T14:36:33.344Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:36:30.243Z
---

# TMG Firewallregeln

# <span class="mw-headline" id="bkmrk-webdav-mit-ssl-in-dm-1">WebDAV mit SSL in DMZ</span>

<div class="vector-body" id="bkmrk-publishing-regel-ers"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-publishing-regel-ers-1" lang="de"><div class="mw-parser-output">1. Publishing Regel erstellen mit folgenden Einstellungen
2. Reiter Nach: Zielserver angeben mit IP Adresse, Ursprünglichen Hostheader weiterleiten anhaken, Ursprung ist der ursprüngliche Client
3. Linkübersetzung: Linkübersetzung für diese Regel anwenden
4. Authentifizierungsdeligierung: Keine Delegierung aber direkte Authentifiezierung des Clients
5. Öffentlicher Name: Hier den Namen angeben auf dem von außen zugegriffen wird.
6. Pfade: wie intern und das Virtuelle Verzeichnis angeben z.B. " /VirtuellesVerzeichnis "
7. Bridging: Anforderung an SSL-Port umleiten mit Standardport (443)
8. Benutzer: Alle Benutzer hinzufügen
9. Listner: 
    1. Netzwerke: Entweder Extern oder eine direkte IP angeben
    2. Verbindungen: SSL Verbindung 443
    3. Zertifikate: Jeder IP ein Zertifikat zuweisen (Ausgestelltes Serverzertifikat für die Externe Adresse)
    4. Authentifizierung: Keine Authentifizierung

</div></div></div>