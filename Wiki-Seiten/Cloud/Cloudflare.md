---
title: Cloudflare
description: 
published: true
date: 2025-01-24T16:12:20.753Z
tags: cdn, dns, authentication, hosting, proxy, vpn
editor: markdown
dateCreated: 2025-01-24T16:10:29.447Z
---

# Entra-ID Authentifizierung
## Einrichtung
Die Einrichtung der Zero Trust Entra-ID Authentifizierung ist hier sehr gut beschrieben.
https://developers.cloudflare.com/cloudflare-one/identity/idp-integration/entra-id/

## Aktualisierung der Vertrauensstellung
Folgend eine Beschreibung falls der Geheime Schlüssel für die Vertrauensstellung abgelaufen ist.
1. In den Cloudflare Zero Trust Einstellungen > Authentifizierung bei den Anmeldemethoden unter Azure AD auf Bearbeiten klicken.
![cloudflare-entraid_001.png](/media/cloudflare-entraid_001.png)
2. In Entra ID unter der App-Registrierung die Cloudflare Authentifizierungs App suchen und hier auf ***Zertifikate & Geheimnisse*** klicken.
![cloudflare-entraid_002.png](/media/cloudflare-entraid_002.png)
3. Einen neuen geheimen Schlüssel ausstellen
![cloudflare-entraid_002.png](/media/cloudflare-entraid_003.png)
4. Schlüssel kopieren
![cloudflare-entraid_002.png](/media/cloudflare-entraid_004.png)
5. Im Cloudflare Portal den ***App-Secret*** Schlüssel ersetzen.
![cloudflare-entraid_002.png](/media/cloudflare-entraid_005.png)
6. Abschließend mit dem Test Knopf einen Verbindungstest durchführen