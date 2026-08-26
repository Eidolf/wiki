---
title: RDS - VDI
description: Remote Services von Microsoft Session basiert oder ein kompletter Desktop
published: true
date: 2026-08-26T19:04:31.332Z
tags: rdp, vdi, rds, remote
editor: markdown
dateCreated: 2026-08-26T18:55:30.049Z
---

# Lizenzierung ohne Session Broker
Konfigurieren Sie die Rolle Remote Desktop Session Host so, dass der lokale Remote Desktop-Lizenzserver verwendet wird. Gehen Sie dazu wie folgt vor:
 
1. Öffnen Sie ein Windows PowerShell-Fenster mit erhöhten Rechten (Als Administrator ausführen).
2. Geben Sie an der PowerShell-Eingabeaufforderung den folgenden Befehl ein und drücken Sie anschließend Eingabetaste:
    `$obj = gwmi -namespace "Root/CIMV2/TerminalServices" Win32_TerminalServiceSetting`
3. Führen Sie den folgenden Befehl aus, um den Lizenzierungsmodus festzulegen.
    `$obj.ChangeMode(value)`
    >  Hinweis: Wert 2 steht für Pro Gerät (Per Device), Wert 4 für Pro Benutzer (Per User).
		{.is-info}
4. Führen Sie den folgenden Befehl aus und ersetzen Sie dabei den Servernamen durch den Namen Ihres Lizenzservers:
    `$obj.SetSpecifiedLicenseServerList("LicServer")`
5. Führen Sie den folgenden Befehl aus, um die in den vorherigen Schritten konfigurierten Einstellungen zu überprüfen:
    `$obj.GetSpecifiedLicenseServerList()`
    
In der Ausgabe sollte nun der konfigurierte Lizenzserver angezeigt werden.
