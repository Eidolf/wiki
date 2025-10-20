---
title: erstellen-eines-san-zertifikats-multiname
description: 
published: true
date: 2023-12-31T14:30:52.569Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:30:49.226Z
---

# Erstellen eines SAN Zertifikats ( Multiname )

# <span class="mw-headline" id="bkmrk-beschreibung%3A">Beschreibung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Erstellen_eines_SAN_Zertifikats_(_Multiname_)&action=edit&section=1 "Abschnitt bearbeiten: Beschreibung:")<span class="mw-editsection-bracket">\]</span></span>

Für die Erstellung eines SAN Zertifikats an einer Microsoft Zertifizierungsstelle muss erstens eine neue Vorlage bereitgestellt werden und zweitens zum einfachen Ausstellen die Berechtigung für die Management Konsole gesetzt werden.

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Erstellen_eines_SAN_Zertifikats_(_Multiname_)&action=edit&section=2 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-zertifizierungsstell"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Zertifizierungsstellen Verwaltung öffnen
2. Zertifizierungsvorlagen bearbeiten
3. Aus Web Server Zertifikat eine doppelte Vorlage erstellen
4. Unter dem Reiter Sicherheit einen Verwaltungscomputer hinzufügen, an dem das Zertifikat ausgestellt werden soll
5. Zertifikat Bereitstellen

</div></div></div># <span class="mw-headline" id="bkmrk-hinweis%3A">Hinweis:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Erstellen_eines_SAN_Zertifikats_(_Multiname_)&action=edit&section=3 "Abschnitt bearbeiten: Hinweis:")<span class="mw-editsection-bracket">\]</span></span>

Nun kann an dem eingetragenen Computer das SAN Zertifikat über die MMC erstellt werden.

# <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Erstellen_eines_SAN_Zertifikats_(_Multiname_)&action=edit&section=4 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://www.carbonwind.net/blog/post/Quick-Dirty-Trick-e28093-Enroll-a-web-server-certificate-from-an-Enterprise-CA(installed-on-Windows-Server-2008-SP2)-using-the-mmc-on-a-Windows-Server-2008-SP2-or-Windows-7-RC-domain-member-machine.aspx" rel="nofollow">http://www.carbonwind.net/blog/post/Quick-Dirty-Trick-e28093-Enroll-a-web-server-certificate-from-an-Enterprise-CA(installed-on-Windows-Server-2008-SP2)-using-the-mmc-on-a-Windows-Server-2008-SP2-or-Windows-7-RC-domain-member-machine.aspx</a>
```