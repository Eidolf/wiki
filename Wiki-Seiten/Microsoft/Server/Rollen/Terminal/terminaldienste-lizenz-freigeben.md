---
title: terminaldienste-lizenz-freigeben
description: 
published: true
date: 2023-12-31T14:37:11.349Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:37:07.911Z
---

# Terminaldienste Lizenz freigeben

Wenn ein Client eine Terminalserverlizenz freigeben soll muss folgender untenstehender Ablauf eingehalten werden.

  
So sichern und löschen Sie den Registrierungsschlüssel MSLicensing

<div class="vector-body" id="bkmrk-navigieren-sie-auf-d"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-navigieren-sie-auf-d-1" lang="de"><div class="mw-parser-output">1. Navigieren Sie auf dem Client zu dem folgenden Registrierungsunterschlüssel: HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\MSLicensing.
2. Klicken Sie auf MSLicensing.
3. Klicken Sie im Menü Registrierung auf Registrierungsdatei exportieren.
4. Geben Sie im Feld Dateiname den Namen mslicensingbackup ein, und klicken Sie dann auf Speichern.
5. Falls Sie diesen Registrierungsschlüssel zu einem späteren Zeitpunkt wiederherstellen müssen, doppelklicken Sie auf mslicensingbackup.reg.
6. Klicken Sie im Menü Bearbeiten auf Löschen und anschließend auf Ja, um das Löschen des Registrierungsunterschlüssels MSLicensing zu bestätigen.
7. Schließen Sie den Registrierungs-Editor und starten Sie anschließend den Client neu.
8. Bei MS Clearinghouse muss angerufen werden um die Lizenz freigeben zu lassen.

</div></div></div>