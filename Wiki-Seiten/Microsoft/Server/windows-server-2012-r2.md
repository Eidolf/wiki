---
title: windows-server-2012-r2
description: 
published: true
date: 2023-12-31T11:55:56.863Z
tags: 
editor: markdown
dateCreated: 2023-12-31T11:55:53.927Z
---

# Windows Server 2012 R2

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span class="mw-headline" id="bkmrk-windows-update-1">Windows Update</span>

### <span class="mw-headline" id="bkmrk-kb4054566-1">KB4054566</span>

Das Update lässt sich durch ein anderes Update (**KB4338419**) nicht installieren.  
Der Download für das **KB4338419** ist folgender [KB4340558](https://www.catalog.update.microsoft.com/Search.aspx?q=KB4340558).  
Wenn man auf Herunterladen klickt, steht das richtige zur Auswahl.  
Der Ablauf ist erst versuchen das Update zu installieren, dann deinstallieren und wiederum danach wieder installieren.

```
expand -f:* “C:\Masters\windows8.1-kb4338419-x64_4d257a38e38b6b8e3d9e4763dba2ae7506b2754d.msu” C:\Masters\expanded\

dism /online /add-package /packagepath:C:\Masters\expanded\Windows8.1-KB4338419-x64.cab

dism /online /remove-package /packagepath:C:\masters\expanded\Windows8.1-KB4338419-x64.cab

dism /online /add-package /packagepath:C:\Masters\expanded\Windows8.1-KB4338419-x64.cab
```

Danach einen Neustart durchführen.

#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://www.borncity.com/blog/2018/07/20/net-framework-update-kb4340558-revidiert/#comment-60825" rel="nofollow">https://www.borncity.com/blog/2018/07/20/net-framework-update-kb4340558-revidiert/#comment-60825</a>
```