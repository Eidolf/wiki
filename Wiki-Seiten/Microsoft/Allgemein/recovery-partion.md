---
title: Recovery Partition
description: Infos und Anpassungen zur Windows Recovery Partition
published: true
date: 2025-12-15T17:59:29.282Z
tags: partition, recovery, windows client, windows server, extend, erweitern
editor: markdown
dateCreated: 2025-12-15T17:59:29.282Z
---

# Beschreibung
Die Windows Recovery-Partition ist nicht unbedingt nötig für den täglichen Betrieb, aber sie ist sehr nützlich, da sie die Windows Wiederherstellungsumgebung (WinRE) enthält, um Startprobleme zu beheben, Systemwiederherstellungen durchzuführen oder Windows zurückzusetzen, ohne ein Installationsmedium zu benötigen. Sie können die Partition löschen, verlieren dann aber die bequeme, integrierte Möglichkeit zur Systemwiederherstellung, müssen dafür aber auf einen externen USB-Stick oder eine DVD zurückgreifen.

# Partition erweitern die durch Recovery Disk blockiert ist
Grundsätzlich kann man mit Third Party Partitionierungs Tools die Recovery Disk vor die primäre Festplatte schieben, es gibt aber eine schnelle und grobe Version mit Diskpart.
In folgender Beschreibung wird die Recovery Disk entfernt, man könnte diese danach wieder anlegen falls 100MB Speicher zur freien Verfügung stehen.
Die Anleitung ist inkl. dem wiederanlegen, aber ich nutze es um eine VM zu erweitern auf der auch ohne Probleme eine Disk angebunden werden kann.

1. Recovery deaktivieren: CMD oder PowerShell als Admin öffnen und mit `reagentc /disable` Wiederherstellungs Partition deaktivieren.
2. Diskpart öffnen: `diskpart` in der CMD eingeben.
3. Die Festplatte identifizieren und auswählen: 
3.1. `list disk` 
3.2. Mit `select disk X` die Windows Festplatte auswählen.
4. Die Wiederherstellungs Partition identifizieren und auswählen: 
4.1. `list partition`
4.2. Mit `select partition X` die Wiederherstellungspartition auswählen.
5. Die Partition mit `delete partition override` löschen.
6. Mit `exit` Diskpart schließen 
7. Jetzt kann die Festplatte C: über Disk Management erweitert werden
8. Recovery wieder aktivieren: In der Admin CMD, `reagentc /enable` ausführen. (Voraussetzung es ist freier Speicher zur Verfügung)