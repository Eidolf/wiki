---
title: IIS
description: Der Microsoft Webserver
published: true
date: 2026-07-25T11:34:20.683Z
tags: 
editor: markdown
dateCreated: 2023-12-31T11:55:31.466Z
---

# Remotezugriff einrichten

1. Voraussetzung ist das der Webserver schon eingerichtet ist
2. `Install-WindowsFeature Web-Mgmt-Service`
3. `Set-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\WebManagement\Server -Name EnableRemoteManagement -Value 1`
4. Webmanagement Dienst neu starten 
    1. Net Stop WMSVC
    2. Net Start WMSVC
5. Evtl. Firewallausnahme erstellen

## Quelle:

http://www.sherweb.com/blog/manage-and-install-iis8-on-windows-2012-server-core/