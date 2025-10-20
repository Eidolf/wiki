---
title: windows-usb-bootstick
description: 
published: true
date: 2023-12-31T13:29:39.481Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:29:36.410Z
---

# Windows USB Bootstick

<div class="vector-body" id="bkmrk-usb-stick-mit-8gb-ei"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-usb-stick-mit-8gb-ei-1" lang="de"><div class="mw-parser-output">1. USB Stick mit 8GB einstecken
2. CMD als Administrator öffnen
3. `diskpart` eingeben
4. `list disk` eingeben
5. Den USB Stick auswählen mit `select disk <strong>Nummer-von-USB-Stick</strong>`
6. Den USB Stick bereinigen mit `clean`
7. Eine bootbare Partition erstellen mit `create Partition Primary`
8. Die erstellte Partition auswählen `select Partition 1`
9. Aktiv schalten `active`
10. Die Partition formatieren `Format fs=fat32 quick`
11. Mit `assign` dem USB Stick einen Buchstaben zuweisen
12. Zum Schluß einfach aus der gewünschten Windows ISO alle Dateien rüber kopieren auf den USB Stick
13. Da es bei FAT32 eine maximale Dateigröße von 4GB das erstellen von großen Install.wim Dateien verhindert kann durch folgende zwei Möglichkeiten das Problem umgangen werden. 
    1. Die einfache Version mit folgendem Tool das Bootbare Medium zu erstellen [Rufus](https://rufus.akeo.ie/)
    2. Unter folgendem [Link](http://blogs.technet.com/b/askcore/archive/2013/03/20/creating-bootable-usb-drive-for-uefi-computers.aspx) Option 2 befolgen.

</div></div></div>