---
title: webdav-einrichten
description: 
published: true
date: 2023-12-31T14:37:35.133Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:37:31.939Z
---

# WebDAV einrichten

# <span class="mw-headline" id="bkmrk-iis6-1">IIS6</span>

<div class="vector-body" id="bkmrk-entweder-unter-der-s"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Entweder unter der Standardwebsite arbeiten oder eine eigene Website anlegen. Bei einer eigenen Website sollte noch ein Ordner im Explorer angelegt werden. In dieser Anleitung wird es nur als Standardwebsite bezeichnet.
2. Eigenschaften der Standardwebsite öffnen.
3. SSL Port angeben (Standard 443)
4. Reiter Verzeichnissicherheit auswählen
5. Unter Authentifizierung und Zugriffsteuerung überprüfen ob der Anonyme Zugriff gewährt ist.
6. Unter Sichere Kommunikation auf Serverzertifikat klicken und ein Zertifikat ausstellen für den Server wenn noch keins vorhanden ist. Die Anforderung bei einer Zertifizierungsstelle einreichen und danach das ausgestellte Zertifikat hier importieren (Vorgang abschließen)
7. Unter der Standardwebsite ein Virtuelles Verzeichnis anlegen
8. Berechtigungen des Virtuellen Verzeichnisses öffnen
9. Internetgastkonto nur Leserechte
10. Andere Benutzer gewollt Rechte verteilen und bestätigen.
11. Eigenschaften des Virtuellen Verzeichnisses öffnen
12. Unter dem Reiter Virtuelles Verzeichnis die Rechte auf Lesen / Schreiben / Verzeichnis durchsuchen / Besuche protokollieren setzen
13. Reiter Verzeichnissicherheit auswählen
14. Bei Authentifizierung und Zugriffsteuerung auf Bearbeiten klicken
15. Anonymen Zugriff deaktivieren und nur Standardauthentifizierung aktivieren.

</div></div></div># <span class="mw-headline" id="bkmrk-webdav-tmg-regel-1">WebDAV TMG Regel</span>

Siehe hier [WebDAV mit SSL in DMZ](https://wiki.eidolf.de/index.php/Firewallregeln#WebDAV_mit_SSL_in_DMZ "Firewallregeln")