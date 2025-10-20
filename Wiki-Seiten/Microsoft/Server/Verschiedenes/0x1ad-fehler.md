---
title: 0x1ad-fehler
description: 
published: true
date: 2023-12-31T13:35:54.922Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:35:51.932Z
---

# 0x1AD Fehler

<div class="vector-body" id="bkmrk-bei-server-2008-auf-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Bei Server 2008 
    - Auf betroffenen Domaincontroller die OOMADs.msi installieren
    - Zu finden unter SCOM Installationsverzeichnis --&gt; HelperObjects --&gt; x64 oder i386 --&gt; OOMADs.msi
    - Nach installation den "System Center Manager" Dienst neu starten

</div></div></div>Bei einer Pushinstallation auf den betroffenen Domaincontroller, wird dieses Paket automatisch mitinstalliert. Bei einer manuellen installation muss das Paket nachinstalliert werden (siehe obrige Punkte)