---
title: dpm-voraussetzungen-ubersicht
description: 
published: true
date: 2023-12-31T14:32:33.941Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:32:30.522Z
---

# DPM Voraussetzungen Übersicht

Die RTM des DPM 2010 ist bereits seit Mitte April fertig und es gibt sie auch als Eval-Version zum freien Download. Es gibt einiges über DPM zu wissen, hier der Versuch eines raschen Überblicks über die wichtigsten Systemvoraussetzungen und weitere Informationen zum DPM 2010!

DPM 2010 Download:

<div class="vector-body" id="bkmrk-verf%C3%BCgbar-nur-als-x6"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Verfügbar nur als x64
- In den “Main Languages”; also Englisch, Deutsch &amp; Co.
- Laden Sie Microsoft System Center Data Protection Manager 2010 noch heute herunter
- Bald im technet und msdn (Verfügbarkeit siehe auch The Right Tools for the Job = SCE 2010 + DPM 2010 und DPM 2010 on MVLS)

</div></div></div>DPM 2010 Voraussetzungen und Planung:

<div class="vector-body" id="bkmrk-hardware%3A-cpu-%3E-1ghz"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Hardware: CPU &gt; 1GHz, &gt; 2GB RAM (empfohlen: 2GHz, 4GB RAM – das macht alleine wegen der SQL Reporting Services Sinn…)
- DPM-Installation: 410MB Program Files, 900 MB DB, 2,7GB Systemlaufwerk. Hardwareanforderungen
- Speicherpool: Das 1,5 fache (empfohlen: das 2-3 fache) der geschützten Dateien, siehe auch Planning the Storage Pool.
- Als Speicherpool können neben lokalen Festplatten auch folgende Speichersysteme verwendet werden: <dl><dd>Direct attached storage (DAS)</dd><dd>Fibre Channel storage area network (SAN)</dd><dd>iSCSI storage device</dd></dl>

</div></div></div>(wie unter Windows üblich: Max. 2TB für MBR Datenträger, max. 17TB für GPT Dynamische Datenträger)

<div class="vector-body" id="bkmrk-dpm-muss-auf-einem-d"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- DPM muss auf einem dedizierten Server ausgeführt werden, der nur einen Zweck erfüllt und bei dem es sich nicht um einen Domänencontroller oder um einen Anwendungsserver handeln darf. Betriebssystemanforderungen für DPM-Server
- Für den DPM-Server ist die permanente Konnektivität mit den geschützten Servern und Desktopcomputern erforderlich – ist wohl klar… Netzwerkanforderungen
- Der DPM-Server darf nicht der Verwaltungsserver für Microsoft Operations Manager (MOM) oder Microsoft System Center Operations Manager sein.
- DPM kann auch als virtuelle Maschine laufen und VHDs nutzen (klar, der Vollständigkeit halber hier nochmals erwähnt, aber:)
- Ein Praxistipp: Damit DPM 2010 in virtuelle Maschinen (VHD-Files) “hineinschauen” und Files daraus restoren kann, empfiehlt Microsoft, den DPM auf einem Windows-Server mit aktivierter Hyper-V Role zu installieren. Dann kann diese Fähigkeit von DPM 2010 genutzt werden.

</div></div></div>DPM 2010 Installation:

<div class="vector-body" id="bkmrk-ev.-m%C3%BCssen-sie-vor-d"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Ev. müssen Sie vor der Installation KB940349 installieren: Softwareanforderungen für DPM-Server – Dieser Hinweis war für DPM 2007. Mal sehen, ob das bei DPM 2010 tatsächlich erforderlich ist…
- DPM installiert die erforderlichen Software-Pakete WDS, .NET 2.0, IIS, etc. Generell empfehlenswert ist, die Role “Anwendungsserver” zuvor zu installieren. Sofern kein eigener SQL Server verwendet wird, schließt DPM die Standard Edition von SQL Server 2008 und SQL Reporting Services ein.
- Vor der Installation von DPM müssen Sie sich als Domänenbenutzer, der der lokalen Administratorgruppe angehört, am Computer anmelden.
- Nach der Installation von DPM müssen Sie als Domänenbenutzer mit Administratorzugriff angemeldet sein, um die DPM-Verwaltungskonsole verwenden zu können. Sicherheitsanforderungen

</div></div></div># <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span>

```
<a class="external free" href="http://blogs.technet.com/b/austria/archive/2010/05/22/system_2d00_center_2d00_data_2d00_protection_2d00_manager_2d00_2010_2d00_uberblick.aspx" rel="nofollow">http://blogs.technet.com/b/austria/archive/2010/05/22/system_2d00_center_2d00_data_2d00_protection_2d00_manager_2d00_2010_2d00_uberblick.aspx</a>
```