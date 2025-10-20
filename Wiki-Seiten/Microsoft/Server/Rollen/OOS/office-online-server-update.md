---
title: office-online-server-update
description: 
published: true
date: 2025-05-03T13:24:54.159Z
tags: 
editor: markdown
dateCreated: 2023-12-31T11:55:49.577Z
---

# Office Online Server update

Da mir bei normalen Windows Server Updates der Office Online Server und sein Vorgänger öfters abgeraucht sind, halte ich mich doch langsam an den Microsoft Hinweis vor den Updates die Farm zu löschen.  
Bedeutet folgenden Ablauf bei ALLEN Updatevorgängen an einem Office Online Server.  
Vorsicht! Alle speziellen Einstellungen gehen verloren, wenn also viel mehr eingestellt wurde als "EditingEnabled", dann sollte man ein Backup der Einstellungen ziehen.
1. Powershell öffnen
	`Remove-OfficeWebAppsMachine`
2. Updates installieren
3. Neustart durchführen
4. Farm erneut erstellen (Nur ein Beispiel)
	`New-OfficeWebAppsFarm -InternalURL "https://oos.domäne.de" -ExternalURL "https://oos.domäne.de" -CertificateName "oos.domäne.de" -EditingEnabled` 