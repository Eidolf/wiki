---
title: Windows 11
description: 
published: true
date: 2024-08-01T15:37:45.759Z
tags: 
editor: markdown
dateCreated: 2024-08-01T15:18:21.388Z
---

# Installation
## Installation auf alten Systemen
1. Registry Datei **BypassCheck.reg** erstellen mit folgendem Inhalt
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\PCHC]
"UpgradeEligibility"=dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\Setup\LabConfig]
"BypassTPMCheck"=dword:00000001
"BypassSecureBootCheck"=dword:00000001
"BypassRAMCheck"=dword:00000001
"BypassStorageCheck"=dword:00000001
"BypassCPUCheck"=dword:00000001
"BypassDiskCheck"=dword:00000001
```
2. Diese Datei wird später der Installation zur Verfügung gestellt, somit auf einem USB Laufwerk oder anderweitig einbinden
3. Windows 11 Installation starten
4. So lange den Wizard durchklicken bis die Aussage erscheint, das der Rechner nicht kompatibel ist,<br>ab diesem Zeitpunkt einen Schritt zurückspringen in Welchem man die Windows Edition auswählt.
5. `Shift + F10` drücken um ein CMD Fenster zu öffnen
6. Notepad.exe eingeben um Notepad zu öffnen
7. In Notepad Datei > öffnen drücken und im Datei Auswahlmenü, erstens die Datei finden und zweitens nach einem Rechtklick,<br>die Datei mit **"Zusammenführen"** bestätigen und Regwerte installieren.
8. Danach in das Installationsfenster zurückwechseln und mit der Installation fortfahren.

Eine ausführliche Beschreibung gibt es in der Quelle.
### Quelle:
https://www.deskmodder.de/wiki/index.php?title=Windows_11_auch_ohne_TPM_und_Secure_Boot_installieren

## Inplace Upgrade auf alten Systemen
1. Das Windows ISO laden und auf dem Zielsystem bereitstellen
2. Eine CMD öffnen und in den Pfad des Installationsmedium wechseln.
3. Mit folgendem Befehl das Setup starten<br>`setup.exe /product server`
4. Hiermit startet der Installationswizard ohne die Kompatibilitätsabfrage
### Quelle:
https://www.deskmodder.de/wiki/index.php?title=Windows_11_auch_ohne_TPM_und_Secure_Boot_installieren

