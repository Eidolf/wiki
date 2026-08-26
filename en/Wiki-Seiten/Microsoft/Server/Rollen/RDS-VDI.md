---
title: RDS - VDI
description: Remote Services from Microsoft, Session Based or complete Desktop
published: true
date: 2026-08-26T19:06:28.550Z
tags: rdp, vdi, rds, remote
editor: markdown
dateCreated: 2026-08-26T18:59:19.204Z
---

# Licensing without Session Broker
Configure the Remote Desktop Session Host role  to use the local Remote Desktop Licensing server. To do this, follow these steps:
 
1. Open an elevated Windows PowerShell Command Prompt window.
2. Type the following command on the PS prompt, and then press Enter:
    `$obj = gwmi -namespace "Root/CIMV2/TerminalServices" Win32_TerminalServiceSetting`
3. Run the following command to set the licensing mode.
    `$obj.ChangeMode(value)`
     >  Note Value = 2 for per device, Value = 4 for per user.
		 {.is-info}
4. Run the following command to replace the machine name with License Server:
    `$obj.SetSpecifiedLicenseServerList("LicServer")`
5. Run the following command to verify the settings that are configured using previous steps:
    `$obj.GetSpecifiedLicenseServerList()`

You should see the server name in the output.
