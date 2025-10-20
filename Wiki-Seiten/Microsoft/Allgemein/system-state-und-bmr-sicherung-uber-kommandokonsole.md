---
title: system-state-und-bmr-sicherung-uber-kommandokonsole
description: 
published: true
date: 2023-12-31T14:29:12.272Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:29:09.246Z
---

# System State und BMR Sicherung über Kommandokonsole

Zum überprüfen ob eine System State Sicherung oder eine Bare Metal Recovery Sicherung funktionieren muss man folgendes in die Kommandokonsole eingeben.

System State Backup:

```
wbadmin start systemstatebackup -backupTarget:<VolumeName>
```

Bare Metal Recovery:

```
wbadmin start backup -allcritical -backuptarget:\\<Servername>\<Freigabenname>
```