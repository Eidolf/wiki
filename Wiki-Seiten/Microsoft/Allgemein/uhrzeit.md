---
title: uhrzeit
description: 
published: true
date: 2025-10-13T14:01:05.406Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:29:17.839Z
---

# Uhrzeit

## Zeitzone konfigurieren
### Zeitzone mit PowerShell
`
Set-TimeZone -Name "Central European Standard Time"
`

# Zeitsynchronisierung

## Regeintrag für w32time

`
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters
`

<table border="1" class="wikitable"><caption>Parameter</caption>
<tbody>
<tr><th>Wertname</th><th> </th><th>Datentyp</th><th> </th><th>Beschreibung !</th></tr><tr><td>Type</td><td> </td><td>REG\_SZ</td><td> </td><td>Wird verwendet, um zu steuern, wie ein Computer synchronisiert wird. Nt5DS = Synchronisierung nach Domänenhierarchie (Standard)

NTP = Synchronisierung nach manuell konfigurierter Quelle

NoSync = Keine Synchronisierung der Zeit

Bei dem Wert "Nt5DS" kann keine manuell konfigurierte Quelle verwendet werden.
</td></tr>
</tbody>
</table>

### Quelle:
http://www.winfaq.de/faq_html/Content/tip1500/onlinefaq.php?h=tip1749.htm

## PDC Zeitgeber manuell eingeben

Prüfen welche Konfiguration aktuell hinterlegt ist.
`w32tm /query /configuration`

Um die aktuell hinterlegten Server anzuzeigen kann außerhalb der Konfiguration noch folgender Befehl helfen.
`w32tm /query /status`

Um einen PDC seinen Zeitgeber Manuell zuzuweisen folgendes eingeben:
`w32tm /config /manualpeerlist:"peer1, peer2" /syncfromflags:manual /reliable:yes /update`
> *Fertiges Beispiel mit NTP Deutschland Pool (kann direkt übernommen werden):
> `w32tm /config /manualpeerlist:"de.pool.ntp.org" /syncfromflags:manual /reliable:yes /update`*
{.is-info}


Im Falle einer Übergabe der PDC FSMO Rolle, kann es sein das der alte PDC weiterhin dazwischen funkt.
Deshalb müssen am alten PDC folgende Befehle ausgeführt werden (manche Befehle sind zum forcieren vorhanden und deshalb redundant)
```
w32tm /config /syncfromflags:domhier /reliable:no /update
net stop w32time
net start w32time
w32tm /config /syncfromflags:domhier /update
w32tm /resync
```

### Quelle:
http://technet.microsoft.com/en-us/library/cc786897(WS.10).aspx
http://technet.microsoft.com/en-us/library/cc773013(WS.10).aspx
http://blog.kopfteam.de/timeserver-sauber-konfigurieren-neue-methode/

# Zeitdienstprobleme

Wenn der net start w32time nicht funktioniert da eine Fehlermeldung angezeigt wird das er eine Dateien nicht finden kann. Als erstes w32tm neu registrieren.
- w32tm /unregister
- w32tm /register

## Quelle:
http://blogs.technet.com/b/industry_insiders/archive/2006/08/29/w32-tm-service.aspx