---
title: Exchange 2019
description: Alles rund um den Exchange Server 2019
published: true
date: 2025-11-03T15:49:20.068Z
tags: microsoft, exchange, office365, e-mail
editor: markdown
dateCreated: 2025-07-18T16:21:56.178Z
---

# Installation
> Auch bei dieser Anleitung zur Exchange 2019 Installation ist es inkl. Upgrade von einer Exchange 2016 Umgebung und somit etwas gemischt vom Aufbau.
> Zum Zeitpunkt an dem sie geschrieben wird ist sie eigentlich veraltet, da der Support auch für 2019 im Oktober 2025 endet.
> Eigentlich entsteht die Anleitung nur, da es eine Vorbereitung für das Upgrade auf Exchange SE ist, den ich privat aber nicht mehr verwenden werde.
> Ich gehe zwar auf die korrekten Voraussetzungen ein, werde aber schlussendlich keine Empfohlenen Werte nehmen. Stellt einfach viel Ressourcen für den Exchange bereit, Microsoft Produkte werden wieder extrem hungrig.
{.is-info}

## Voraussetzungen

### Festplatten
Es sollten getrennte Festplatten für folgende Bereiche existieren
- Betriebssystem (C:) 80GB - NTFS
- Exchange Anwendung (E:) 130GB - NTFS
- Exchange Datenbanken und Datenbank Logs (F:) 130GB - ReFS 
- Exchange Anwendung / Web Logs (G:) 30GB - ReFS
- Exchange Queue (H:) 20GB - ReFS
> Dies ist in meinen Augen die mindest Aufteilung um eine einigermaßen gute Geschwindigkeit zu bekommen.
> Für einen Firmeneinsatz würde ich aber noch weiter aufteilen z.B. pro Datenbank eine Platte und dazugehörige Logs ausgliedern.
> Weiterhin die Logs zwischen Anwendung und Web-Diensten trennen.
> Ausführlich bei Microsoft zu finden https://learn.microsoft.com/de-de/exchange/plan-and-deploy/deployment-ref/storage-configuration
{.is-warning}

![ex2019-install-001.png](/media/ex2019-install-001.png)

### Pagefile
Bei einem Exchange 2019 sollte das Pagefile manuell gesetzt werden und nicht durch das System, automatisch verwaltet werden.
Das Pagefile sollte 25% des Arbeitsspeichers haben.
Bei **64GB** Arbeitsspeicher somit **16GB**

#### Quelle:
https://www.alitajran.com/configure-pagefile-exchange-server/

### Software und Rollen
Microsoft schlägt selbst vor eine Server Core Installation für den Exchange zu verwenden, mir ist es aber zu heikel bzw. zu umständlich im Fehlerfall nicht alle Tools direkt auf dem Server zu haben.

**.Net Framework 4.8** ist standardmäßig installiert und unterstützt.
<sub>siehe MS Unterstützungsmatrix > https://learn.microsoft.com/de-de/exchange/plan-and-deploy/supportability-matrix#net-framework</sub>

Für die Rolleninstallation habe ich ein Script welches prüft ob nötigen Rollen schon vorhanden sind oder installiert werden müssen, somit auch für die Nachinstallation von fehlenden Rollen geeignet.

[Exchange-2019-Roles-Features.ps1](https://github.com/Eidolf/Public-Scripts/blob/main/Exchange/Exchange-2019-Roles-Features.ps1){target=_blank}
> Dauert eine gewisse Zeit und kann sehr lange bei einer Prozentzahl stehen bleiben, einfach laufen lassen da es keine Einzelüberprüfung der Rollen gibt.
{.is-info}

Folgende Software muss noch installiert werden.
- [Unified Communications Managed API 4.0 Runtime](https://www.microsoft.com/en-us/download/details.aspx?id=34992){target=_blank}
- [Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784){target=_blank}
- [URL Rewrite in 64bit](https://www.iis.net/downloads/microsoft/url-rewrite){target=_blank} (Hierfür sollte der Webserver installiert sein, daher Rollen als erstes installieren)

### Active Directory

Das Active Directory muss typischerweise auf einen neuen Exchange Server vorbereitet werden, zu dem komme ich im nächsten Punkt.
Meine vorhandene AD 2022 Umgebung ist unterstützt.
<sub> siehe MS Unterstützungsmatrix > https://learn.microsoft.com/de-de/exchange/plan-and-deploy/supportability-matrix#supported-active-directory-environments </sub>

> Eine Gesamtstruktur niedriger als 2012 R2 wird nicht unterstützt und muss erstmal auf aktuellen Stand gebracht werden.
{.is-warning}

## AD Vorbereitung
Exchange Installationen benötigen eine AD Schema Erweiterung, diese habe ich unter Exchange 2016 > Update > Punkt 2 beschrieben.
Ich verlinke es hier, da das gleiche Vorgehen ist.
[Exchange-2016#update](/de/Wiki-Seiten/Microsoft/Server/Rollen/Exchange/exchange-2016#update)

## Installation
1. Den Defender / installierten Antivirus Echtzeitschutz beenden.<span style="color:red">
Dies ist keine Empfehlung von mir und jeder sollte selbst entscheiden ob er das Risiko eingeht.
Persönlich hatte ich bei vielen Exchange Installationen durch aktivierte Virenenprogamme hohe zeitliche Verzögerung,
diese Fehlerquelle möchte ich ausschließen.</span>
2. Exchange Installations Wizard starten und Lizenbedingungen akzeptieren (Diagnosedaten werden bei mir deaktiviert)
![exchange2019-001.png](/media/exchange2019-001.png)
![exchange2019-002.png](/media/exchange2019-002.png)
3. Bei "Recommended Settings" wähle ich persönlich auch keine Daten an MS zu übertragen.
![exchange2019-003.png](/media/exchange2019-003.png)
4. Folgende Rollen werden aktiviert
- Mailbox role
- Management tools <sup>(wird automatisch durch die Mailbox Rolle markiert)</sup>
- Automatische installation von Rollen und Funktionen
![exchange2019-004.png](/media/exchange2019-004.png)
5. Installationspfad wählen, bei mir ist es **Laufwerk "E:"**, <br> den Pfad kürze ich nicht ein um bei etwaigen Scripten einfach nur den Laufwerksbuchstaben ändern zu müssen.
![exchange2019-005.png](/media/exchange2019-005.png)
6. Antivirus bleibt aktiviert.
![exchange2019-006.png](/media/exchange2019-006.png)
7. Jetzt werden die Voraussetzungen geprüft und installiert.
![exchange2019-007.png](/media/exchange2019-007.png)
8. Nach der Überprüfung auf installieren klicken
![exchange2019-008.png](/media/exchange2019-008.png)
9. Die Installation abschließen, den Antivirus Echzeitschutz wieder aktivieren und neu starten.
![exchange2019-009.png](/media/exchange2019-009.png)

## Einrichtung
### Zeitzone und Lizenz
1. Grundeinstellungen des Exchange Admin Center festlegen
![exchange2019-010.png](/media/exchange2019-010.png)
2. Die Installation erkennt bestehende Installationen und ist sofort danach als zusätzlicher Exchange ohne Funktion eingebunden.
![exchange2019-011.png](/media/exchange2019-011.png)
3. Zu aller erst hinterlege ich den Lizenzschlüssel
![exchange2019-025.png](/media/exchange2019-025.png)
### Zertifikat
4. In meinem Fall erstelle ich ein neues Zertifikat für den Webserver da es kurz vor dem Ablauf ist, ansonsten das bisherige am neuen Server importieren. Wie ein Zertifikat am Exchange erstellt wird steht zwar überall, aber hiermit habe ich es einmal mit Bildern festgehalten.
4.1. Auf das Plus bei den Zertifikaten klicken
![exchange2019-012.png](/media/exchange2019-012.png)
4.2. Anforderung an Zertifizierungstelle erstellen
![exchange2019-013.png](/media/exchange2019-013.png)
4.3. Den Anzeigenamen festlegen, ich wähle den bisherigen.
![exchange2019-014.png](/media/exchange2019-014.png)
4.4. Bei mir wird es kein Platzhalterzertifikat, somit auf Weiter klicken.
![exchange2019-015.png](/media/exchange2019-015.png)
4.5. Speicherort auf dem Server auswählen, der CSR wird direkt auf dem Exchange abgelegt und nicht unbedingt dort wo die Konsole geöffnet wurde. Im Beispiel bin ich auf Durchsuchen gegangen und habe den neuen Exchange Server **EIDEX02** ausgewählt.
Nachtrag: Der Download der Request Datei ist neuerdings nicht ausschließlich auf dem Server sondern wird abschließend in der geöffneten Browsersitzung initiert. 
![exchange2019-016.png](/media/exchange2019-016.png)
4.6. Bei der Bearbeitung der alternativen Antragstellernamen habe ich es wie das bisherig eingestellt, in meinem Fall, durch Split-DNS alles nur auf die externen Namen.
![exchange2019-017.png](/media/exchange2019-017.png)
![exchange2019-018.png](/media/exchange2019-018.png)
4.7. Organisationsdaten des Zertifikats angeben.
![exchange2019-019.png](/media/exchange2019-019.png)
4.8. Abschließender Hinweis das es sich nur um einen CSR handelt und man diesen an eine CA einreichen muss. Mit dem abschließenden **Fertig stellen** startet der Browser Download der **.req** Datei
![exchange2019-020.png](/media/exchange2019-020.png)
5. Request Datei bei der CA einreichen und die nötigen Zertifikate ausstellen (Die Beschreibung dazu überspringe ich)
6. Die Ausstehende Anforderung abschließen mit dem ausgestellten Zertifikat.
![exchange2019-021.png](/media/exchange2019-021.png)
![exchange2019-022.png](/media/exchange2019-022.png)
7. Danach kann man dem Zertifikat den IIS Dienst zuweisen.
![exchange2019-023.png](/media/exchange2019-023.png)
![exchange2019-024.png](/media/exchange2019-024.png)

