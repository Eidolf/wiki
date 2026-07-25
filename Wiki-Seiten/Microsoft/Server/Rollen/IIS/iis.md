---
title: IIS
description: Der Microsoft Webserver
published: true
date: 2026-07-25T12:12:20.833Z
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

# Fehler
## Webverwaltungsdienst kann nicht gestartet werden
Folgende Einträge kann es in der Ereignisanzeige geben
Im System Log
> Der Dienst "Webverwaltungsdienst" wurde mit dem folgenden dienstspezifischen Fehler beendet: 
> Unbekannter Fehler
> Quelle: Service Control Manager
> ID: 7024
{.is-danger}

Und im Anwendung Log folgender
> Die Beschreibung für die Ereignis-ID "1007" aus der Quelle "Microsoft-Windows-IIS-IISManager" wurde nicht gefunden. Entweder ist die Komponente, die dieses Ereignis auslöst, nicht auf dem lokalen Computer installiert, oder die Installation ist beschädigt. Sie können die Komponente auf dem lokalen Computer installieren oder reparieren.
> 
>Falls das Ereignis auf einem anderen Computer aufgetreten ist, mussten die Anzeigeinformationen mit dem Ereignis gespeichert werden.
>
>Die folgenden Informationen wurden mit dem Ereignis gespeichert: 
>
>IISWMSVC_STARTUP_UNABLE_TO_READ_CERTIFICATE
>
>Das Zertifikat mit Fingerabdruck "" konnte nicht gelesen werden. Stellen Sie sicher, dass das SSL-Zertifikat vorhanden und auf der Seite des Verwaltungsdiensts ordnungsgemäß konfiguriert ist.
>
> Quelle: IIS-IISManager
> ID: 1007
{.is-danger}

### Beschreibung
Es ist mit hoher Wahrscheinlichkeit das Zertifikat abgelaufen oder gelöscht worden und muss neu gesetzt werden.
### Behebung
Am einfachsten die IIS Konsole öffnen und am obersten Server Punkt die Verwaltungsdienst konfiguration öffnen.
![iis-verwaltungdienst-001.png](/media/iis-verwaltungdienst-001.png)
Und in dem Menü beim **SSL-Zertifikat** Eintrag ein Zertifikat hinterlegen.

