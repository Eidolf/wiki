---
title: EWS Abschaltung
description: Hinweise und Vorbereitungen für die Abschaltung von EWS in Exchange Online
published: true
date: 2026-09-14T16:48:06.153Z
tags: exchange, ews, api, graph
editor: markdown
dateCreated: 2026-09-14T16:47:17.470Z
---

# Kurzanleitung: EWSAllowedAppIDs für Exchange Online

## Worum geht es?

Microsoft stellt **Exchange Web Services (EWS)** in Exchange Online schrittweise ein. Ab **Oktober 2026** wird EWS standardmäßig zunehmend deaktiviert.

Mit **EWSAllowedAppIDs** können Administratoren festlegen, **welche Anwendungen EWS weiterhin nutzen dürfen**.

## Wichtigste Änderung ab Oktober 2026

Wenn EWS weiterhin benötigt wird, reicht folgendes künftig **nicht mehr** aus:

```powershell
Set-OrganizationConfig -EWSEnabled $true
```

Ohne konfigurierte **EWSAllowedAppIDs-Liste** wird EWS trotz `EWSEnabled=True` blockiert.

### Verhalten ab Oktober 2026

| EWSEnabled | EWSAllowedAppIDs | Ergebnis |
|------------|------------------|-----------|
| True | Leer oder Null | Alle EWS-Zugriffe blockiert |
| True | Befüllt | Nur freigegebene Apps erlaubt |
| False | Beliebig | Alle EWS-Zugriffe blockiert |

## Empfohlenes Vorgehen

### 1. EWS-Nutzung ermitteln

- Identifizieren, welche Anwendungen noch EWS verwenden.
- Nur tatsächlich benötigte Anwendungen berücksichtigen.
- Wenn möglich, auf Microsoft Graph migrieren.

### 2. Aktuelle Konfiguration prüfen

```powershell
Get-OrganizationConfig -RetrieveEwsOperationAccessPolicy | Format-List EwsAllowedAppIDs
```

### 3. EWS-Allow-Liste erstellen

Beispiel mit zwei erlaubten App-IDs:

```powershell
Set-OrganizationConfig -EwsAllowedAppIDs "11111111-2222-3333-4444-555555555555,aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
```

### 4. Konfiguration kontrollieren

```powershell
Get-OrganizationConfig -RetrieveEwsOperationAccessPolicy | Format-List EwsAllowedAppIDs
```

## App-ID zur Liste hinzufügen

```powershell
# Aktuelle Liste lesen
$current = (Get-OrganizationConfig -RetrieveEwsOperationAccessPolicy | Select-Object -ExpandProperty EwsAllowedAppIDs)

# Neue App-ID definieren
$newAppId = "99999999-8888-7777-6666-555555555555"

# Liste erweitern
$updated = @($current, $newAppId)

# Gesamte Liste zurückschreiben
Set-OrganizationConfig -EwsAllowedAppIDs ($updated -join ",")
```

## App-ID aus der Liste entfernen

```powershell
# Aktuelle Liste lesen
$current = (Get-OrganizationConfig -RetrieveEwsOperationAccessPolicy | Select-Object -ExpandProperty EwsAllowedAppIDs)

# Zu entfernende App-ID
$removeAppId = "99999999-8888-7777-6666-555555555555"

# Liste aufteilen
$appIds = $current -split ","

# App-ID entfernen
$updated = $appIds | Where-Object { $_ -ne $removeAppId }

# Gesamte Liste zurückschreiben
Set-OrganizationConfig -EwsAllowedAppIDs ($updated -join ",")
```

## Wichtige Hinweise

- Änderungen an `EwsAllowedAppIDs` können bis zu **24 Stunden** benötigen, bis sie wirksam werden.
- `Set-OrganizationConfig -EwsAllowedAppIDs` überschreibt immer die **gesamte Liste**.
- Vor Änderungen immer zuerst die bestehende Liste auslesen.
- Die bisherige Funktion **EWSAllowList** arbeitet mit **User Agents**, nicht mit **App-IDs**.
- Für die EWS-Abkündigung ist ausschließlich **EWSAllowedAppIDs** relevant.

## Admin-Checkliste

- [ ] EWS-Nutzung analysieren
- [ ] Benötigte App-IDs erfassen
- [ ] EWSAllowedAppIDs konfigurieren
- [ ] Anwendungen testen
- [ ] Migration auf Microsoft Graph planen
- [ ] Vor Oktober 2026 produktiv validieren

## Minimal-Konfiguration für EWS nach Oktober 2026

```powershell
Set-OrganizationConfig -EwsAllowedAppIDs "AppID1,AppID2,AppID3"
Set-OrganizationConfig -EWSEnabled $true
```

Danach dürfen nur die angegebenen Apps weiterhin über EWS auf Exchange Online zugreifen.

## Quelle:
https://techcommunity.microsoft.com/blog/exchange/introducing-ewsallowedappids-preparing-for-the-final-phase-of-ews-retirement/4529471