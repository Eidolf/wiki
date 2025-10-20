---
title: gp-wmi
description: 
published: true
date: 2023-12-31T14:30:33.591Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:30:30.452Z
---

# GP WMI

# <span class="mw-headline" id="bkmrk-windows-7-abfrage-1">Windows 7 Abfrage</span>

```
select * from Win32_OperatingSystem where Version like "6.1%" and ProductType = "1"
```