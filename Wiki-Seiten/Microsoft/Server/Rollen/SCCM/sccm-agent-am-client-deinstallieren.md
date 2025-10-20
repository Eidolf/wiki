---
title: sccm-agent-am-client-deinstallieren
description: 
published: true
date: 2023-12-31T13:34:40.581Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:34:37.442Z
---

# SCCM Agent am Client deinstallieren

# <span class="mw-headline" id="bkmrk-vor-sccm-2007-1">Vor SCCM 2007</span>

Befehl ausführen **ccmclean**

# <span class="mw-headline" id="bkmrk-ab-sccm-2007-1">Ab SCCM 2007</span>

<div class="vector-body" id="bkmrk-vor-windows-server-2"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Vor Windows Server 2008: **C:\\Windows\\system32\\ccmsetup** ist die **ccmsetup.exe** zu finden.
- Ab Windows Server 2008: **C:\\Windows\\ccmsetup** ist die **ccmsetup.exe** zu finden.

</div></div></div>```
ccmsetup.exe /uninstall
```