---
title: Vertraulichkeitsbezeichnungen
description: Diese Seite befasst sich mit dem Schutz von Daten über Vetraulichkeitsbzeichnungen (Labels) die fest mit Dokumenten übergeben werden.
published: true
date: 2025-08-19T15:59:39.820Z
tags: microsoft, label, pureview, information protection, audit, daten sicherheit, datenschutz
editor: markdown
dateCreated: 2025-08-19T15:59:39.820Z
---

# 📘 Einrichtung: Sensitivity Labels in Microsoft Purview für Gruppen & Sites

Mit **Microsoft Purview Sensitivity Labels** können Sie nicht nur Dateien und E-Mails schützen, sondern auch **Microsoft Teams**, **Microsoft 365-Gruppen**, **SharePoint-Websites** und **Loop-Arbeitsbereiche** absichern. Diese Labels helfen, Datenschutz, Zugriffssteuerung und Compliance-Richtlinien durchzusetzen.

---

## ✅ Voraussetzungen

- Microsoft 365 E3/E5 oder entsprechende Add-ons
- Administratorrechte in **Microsoft Entra ID** (ehemals Azure AD)
- PowerShell-Module:
  - `AzureADPreview`
  - `ExchangeOnlineManagement`

---

## 🔹 1. Aktivieren der Unterstützung für Gruppen & Sites

Standardmäßig ist die Option **Groups & Sites** in Sensitivity Labels deaktiviert. Aktivieren Sie sie wie folgt:

### **Schritte in PowerShell**

```powershell
# 1. AzureADPreview-Modul installieren und importieren
Install-Module AzureADPreview
Import-Module AzureADPreview
Connect-AzureAD

# 2. Prüfen, ob Gruppeneinstellungen vorhanden sind
$grpUnifiedSetting = (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ)
$Setting = $grpUnifiedSetting
$grpUnifiedSetting.Values

# 3. Falls keine Einstellungen vorhanden, Template laden und neue Settings erstellen
$TemplateId = (Get-AzureADDirectorySettingTemplate | where { $_.DisplayName -eq "Group.Unified" }).Id
$Template = Get-AzureADDirectorySettingTemplate | where -Property Id -Value $TemplateId -EQ
$Setting = $Template.CreateDirectorySetting()

# 4. Sensitivity Labels aktivieren
$Setting["EnableMIPLabels"] = "True"

# 5. Änderungen speichern
New-AzureADDirectorySetting -DirectorySetting $Setting  # für neue Settings
# oder
Set-AzureADDirectorySetting -Id $grpUnifiedSetting.Id -DirectorySetting $Setting  # für bestehende Settings
```

## 🔹 2. Labels mit Entra ID synchronisieren

Damit Sensitivity Labels für Gruppen und Sites korrekt funktionieren, müssen sie mit **Microsoft Entra ID** (ehemals Azure AD) synchronisiert werden. Dies stellt sicher, dass die Labels in der gesamten Umgebung verfügbar sind und korrekt angewendet werden können.

### Schritte:

1. **Vergewissern Sie sich**, dass die Unterstützung für Gruppen & Sites aktiviert ist (siehe Schritt 1).
2. **Synchronisation prüfen**:
   - Öffnen Sie das [Microsoft Purview Compliance Portal   - Navigieren Sie zu **Information Protection → Labels**
   - Prüfen Sie, ob die erstellten Labels unter dem Scope **Groups & Sites** sichtbar sind
3. **PowerShell-Validierung (optional)**:
   - Mit dem `ExchangeOnlineManagement`-Modul können Sie prüfen, ob Labels korrekt bereitgestellt wurden:
     ```powershell
     Connect-ExchangeOnline
     Get-Label | Where-Object {$_.ContentType -eq "Site, UnifiedGroup"}
     ```
4. **Warten Sie ggf. bis zu 24 Stunden**, bis neue Labels in Microsoft Teams, SharePoint und Outlook vollständig verfügbar sind.

> 💡 **Hinweis**: Die Synchronisation erfolgt automatisch, kann aber je nach Umgebung verzögert sein. Bei Problemen empfiehlt sich ein manueller Refresh oder ein Neustart der entsprechenden Dienste.


## 🔹 3. Sensitivity Label erstellen (Scope: Groups & Sites)

### Schritte:

1. Gehe zum [Microsoft Purview Compliance Portal](https://purview.microsoft.com/home)
2. Navigiere zu **Information Protection → Labels → Neues Label**
3. Wähle **Scope: Groups & Sites**
4. Konfiguriere die Schutzeinstellungen:

#### 🔐 Privacy:
- **Public** → Jeder in der Organisation kann beitreten
- **Private** → Nur genehmigte Mitglieder

#### 🌐 Externer Benutzerzugriff:
- Gäste **zulassen** oder **blockieren**

#### 📤 Externe Freigabe:
- Nur Organisation  
- Bestehende Gäste  
- Neue & bestehende Gäste  
- Jeder *(vorsichtig verwenden!)*

#### 🔒 Conditional Access:
- Zugriff von **nicht verwalteten Geräten** blockieren oder einschränken

#### 🔑 Authentifizierungskontext:
- Für **strengere Zugriffskontrollen**

---

## 🔹 4. Anwendung der Labels

- **Neue Teams oder Gruppen**: Label wird bei Erstellung ausgewählt
- **Bestehende Gruppen/Sites**: Label kann nachträglich über das Compliance Center oder PowerShell angewendet werden
- **Loop-Arbeitsbereiche**: Werden automatisch durch die Gruppenrichtlinien abgedeckt

---

## 🔍 Wichtige Hinweise
- **Container-Labels** vererben sich **nicht** auf Dateien oder E-Mails  
  → Aktiviere zusätzlich **Sensitivity Labels für Office-Dateien** in SharePoint & OneDrive
- Änderungen an Labels können **Privatsphäre-Einstellungen überschreiben**
- **Shared Channels in Teams** erben automatisch das Label des übergeordneten Teams
