---
title: bitlocker
description: 
published: true
date: 2023-12-31T13:28:38.530Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:28:35.326Z
---

# BitLocker

# <span class="mw-headline" id="bkmrk-pin-abfrage-aktivier-1">PIN Abfrage aktivieren:</span>

<div class="vector-body" id="bkmrk-die-lokale-gruppenri"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Die lokale Gruppenrichtlinienverwaltung öffnen `gpedit.msc`
2. Zu folgendem Pfad wechseln `Computerkonfiguration / Administrative Vorlagen / Windows Komponenten / Bitlocker-Laufwerksverschlüsselung / Betriebssystemlaufwerke`
3. Folgenden Eintrag ändern `Zusätzliche Authentifizierung beim Start anfordern`
4. Haken entfernen bei "Bitlocker ohne kompatibles TPM zulassen"
5. Einstellung setzen `Systemstart-PIN bei TPM zulassen`
6. Neustart durchführen
7. Bitlocker-Verwaltung aufrufen
8. PIN setzen

</div></div></div>## <span class="mw-headline" id="bkmrk-hinweise-1">Hinweise</span>

Die Standardlänge sind 4 Zeichen, kann aber über die Policy "Minimale PIN-Länge für Systemstart konfigurieren" erweitert werden Wenn eine Option auf erzwungen gestellt wird kann es sein das dadurch ein Konflikt ensteht und man deshalb keine Optionen gesetzt werden können.

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://www.tecchannel.de/a/zusaetzliche-pin-abfrage-fuer-bitlocker-beim-windows-start-einrichten,3277851" rel="nofollow">https://www.tecchannel.de/a/zusaetzliche-pin-abfrage-fuer-bitlocker-beim-windows-start-einrichten,3277851</a>
```

# <span class="mw-headline" id="bkmrk-abfrage-des-aktuelle-1">Abfrage des aktuellen Identifiers</span>

Es kommt vor das man ein Laufwerk mehrmals neu verschlüsselt hat und zum Schluss nicht mehr weiß welcher Wiederherstellungsschlüssel aktuell ist.  
Mit folgendem **CMD/PowerShell** Befehl kann man die ID und das zugehörige Entschlüsselungspasswort (Falls entsperrt) anzeigen.  
`manage-bde -protectors -get c:` (pro Laufwerk)

## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://answers.microsoft.com/en-us/windows/forum/all/where-to-find-the-identifier-value-displayed-on-my/78cb5f06-aa73-463e-8f61-a21330f3df3f" rel="nofollow">https://answers.microsoft.com/en-us/windows/forum/all/where-to-find-the-identifier-value-displayed-on-my/78cb5f06-aa73-463e-8f61-a21330f3df3f</a>
```