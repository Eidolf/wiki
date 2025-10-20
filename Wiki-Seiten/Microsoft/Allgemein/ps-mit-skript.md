---
title: ps-mit-skript
description: 
published: true
date: 2023-12-31T14:28:49.563Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:28:46.458Z
---

# PS mit Skript

Um ein Powershellskript auszuführen folgenden Befehl eingeben

```
C:\WINDOWS\system32\WindowsPowerShell\v1.0\powershell.exe -command C:\Users\poweruser\Desktop\Skript.ps1
```

  
Das Skript arbeitet Powershell Befehle nacheinander ab, um z.B. das AD Modul zu laden und danach eine AD Abfrage zu starten muss die "Skript.ps1" Datei folgendermaßen aufgebaut werden.

```
import-module activedirectory 
Search-ADAccount -LockedOut | Unlock-ADAccount
```

  
  
Quelle:

```
<a class="external free" href="http://www.fam-lange.com/index.php/exchange-2007/69-powershell-skript-zeitgesteuert-starten-und-email-report" rel="nofollow">http://www.fam-lange.com/index.php/exchange-2007/69-powershell-skript-zeitgesteuert-starten-und-email-report</a>
<a class="external free" href="http://blogs.msdn.com/b/adpowershell/archive/2009/02/25/ad-powershell-quick-start-guide.aspx" rel="nofollow">http://blogs.msdn.com/b/adpowershell/archive/2009/02/25/ad-powershell-quick-start-guide.aspx</a>
```