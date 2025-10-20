---
title: scom-agent-installieren
description: 
published: true
date: 2023-12-31T13:35:09.409Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:35:06.432Z
---

# SCOM Agent installieren

Folgende Schritte für eine ordnungsgemäße Installation des SCOM Agents.

<div class="vector-body" id="bkmrk-zertifikat-f%C3%BCr-den-g"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-zertifikat-f%C3%BCr-den-g-1" lang="de"><div class="mw-parser-output">1. Zertifikat für den gewünschten Server erstellen und anschließend mit dem Private Key exportieren in eine Datei.
2. Ein Komplettpaket erstellen mit folgenden Inhalt. 
    - MOM Agent mit Updates 32/64 Bit
    - MOMCERTIMPORT.exe 32/64 Bit
    - Root Zertifikat der lokalen Zertifizierungsstelle
3. Das Serverzertifikat und das komplett Paket auf den betreffenden Server schieben.
4. Zertifikat´s MMC öffnen und Computerzerifikate hinzufügen
5. Zu den eigenen Computerzertifikaten das Serverzertifikat importieren
6. Zu den vertrauenswürdigen Stammzertifizierungstellen das Root Zertifikat importieren
7. Agent installieren 
    - Angeben der Domäne des SCOM Servers
    - Angeben des Pfades zum SCOM oder SCOM Gateway Servers
8. In der Zertifikats´s MMC unter Operations Manager überprüfen ob das Richtige Zertifikat vorhanden ist, sonst abändern auf das neue.
9. MOMCERTIMPORT " MOMCERTIMPORT.exe Serverzertifikatsname " ausführen
10. System Center Management Dienst neu starten
11. Auf der System Center Operations Manager Console den hinzugefügten Server noch aktivieren ( zu finden unter Administration --&gt; ausstehende Anforderungen

</div></div></div>