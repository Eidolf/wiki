---
title: sccm-client-fehler
description: 
published: true
date: 2023-12-31T13:34:45.309Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:34:42.107Z
---

# SCCM Client Fehler

# <span class="mw-headline" id="bkmrk-sccm-client-findet-k-1">SCCM Client findet kein Boot File</span>

Wenn der Client gestartet wird, kommt die PXE Copyright Einblendung dann DHCP und danach **PXE-E53: No Boot File Recived**

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.bootix.de/support/problems_solutions/pxe_e53_no_boot_filename_received.html" rel="nofollow">http://www.bootix.de/support/problems_solutions/pxe_e53_no_boot_filename_received.html</a>
```

# <span class="mw-headline" id="bkmrk-sccm-client-findet-t-1">SCCM Client findet TFTP nicht mehr</span>

Wenn der Client gestartet wird, kommt sofort die Fehlermeldung **PXE-T01: File not found**  
Falls sonst alles richtig eingestellt ist (Die Ankündigung hat auch eine Tasksequenz die auch Installationspaket beinhaltet) kann folgender Link in der Quelle weiterhelfen

## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
http://social.technet.microsoft.com/Forums/en-CA/configmgrosd/thread/90308be3-81bc-462b-a014-a10e48452354
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-sccm-client-l%C3%A4uft-in-1">SCCM Client läuft in Wiederherstellung, bricht ab und startet neu</span>

Der SCCM Client läuft zwar in die Wiederherstellung hinein, bricht aber nach kurzer Zeit ab und startet neu. Danach wird die Meldung angezeigt, das kein Bootimage verfügbar ist. In der kurzen Phase der Wiederherstellung wird folgende Meldung im SCCM Log angezeigt.

```
OSD Deployment : 
TSMBOOTSTRAP.exe – Corrupt FILE : 
The file or directory C:\_SMSTaskSequence is corrupt ad unreadable. Please run the CHKDSK utility.
```

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

Mit **Diskpart** alle vorhandenen Partitionen löschen und eine kleine neue anlegen.

## <span class="mw-headline" id="bkmrk-weitere-info%3A-1">Weitere Info:</span>

Der Ordner **C:\\\_SMSTaskSequence** ist auf die Partition schon geschrieben worden, dieser ist aber defekt z.B. durch Ausschalten im Startvorgang. Es fehlen nun die Berechtigungen diesen Ordner auf der Partition zu überschreiben.

## <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="http://scug.be/blogs/sccm/archive/2010/12/14/osd-deployment-tsmbootstrap-exe-corrupt-file-the-file-or-directory-c-smstasksequence-is-corrupt-ad-unreadable-please-run-the-chkdsk-utility.aspx" rel="nofollow">http://scug.be/blogs/sccm/archive/2010/12/14/osd-deployment-tsmbootstrap-exe-corrupt-file-the-file-or-directory-c-smstasksequence-is-corrupt-ad-unreadable-please-run-the-chkdsk-utility.aspx</a>
```