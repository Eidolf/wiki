---
title: netzwerkumgebung
description: 
published: true
date: 2023-12-31T13:29:02.412Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:28:59.178Z
---

# Netzwerkumgebung

## <span class="mw-headline" id="bkmrk-nur-anzeige-von-rech-1">Nur Anzeige von Rechner im gleichen Subnetz</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Netzwerkumgebung&action=edit&section=1 "Abschnitt bearbeiten: Nur Anzeige von Rechner im gleichen Subnetz")<span class="mw-editsection-bracket">\]</span></span>

Problembeschreibung:  
Nach dem Upgrade eines Windows Server 2003 Domäne Controller auf Windows Server 2008 oder beim Beitreten eines Windows Server 2008 Computers ins Netzwerk, stellen Sie fest, dass der Computersuchdienst nicht einwandfrei funktioniert. Die folgenden Symptome können beim Aufbringen der Netzwerkumgebung aufkommen:

<div class="vector-body" id="bkmrk-sie-k%C3%B6nnen-andere-co"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Sie können andere Computer von einem anderen Subnetz nach dem Upgrade nicht mehr sehen.
2. Die Computer Liste in demselben Subnetz kann Inkonsistenzen aufweisen.

</div></div></div>Ursache:  
Dieses Problem kann auftreten, weil der Computersuchdienst standardmäßig in Windows Server 2008 deaktiviert ist.

  
Lösung:  
Um das Problem zu beheben, stellen Sie den Computerbrowser Dienst auf Automatisch und starten den Dienst auf den gewünschten Windows Server 2008 Computer:

<div class="vector-body" id="bkmrk-stellen-sie-sicher%2C-"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-stellen-sie-sicher%2C--1" lang="de"><div class="mw-parser-output">1. Stellen Sie sicher, dass die Datei- und Druckerfreigabe eingeschaltet ist.
2. Klicken Sie auf Start – Ausführen und öffnen Services.msc.
3. Finden Sie den Computerbrowser Dienst und mit einen Rechtsklick die Eigenschaften auswählen.
4. Ändern Sie den Starttyp auf Automatisch.
5. Starten Sie den Dienst.

</div></div></div>