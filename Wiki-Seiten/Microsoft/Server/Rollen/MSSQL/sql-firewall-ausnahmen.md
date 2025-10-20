---
title: sql-firewall-ausnahmen
description: 
published: true
date: 2023-12-31T13:34:17.729Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:34:14.659Z
---

# SQL Firewall Ausnahmen

# <span class="mw-headline" id="bkmrk-standard-datenbank-i-1">Standard Datenbank Instanz</span>

Für die Standard Datenbankinstanz kann man folgende PowerShell Befehle verwenden um Firewallregeln zu erstellen

```
New-NetFirewallRule -DisplayName "SQLServer default instance" -Direction Inbound -LocalPort 1433 -Protocol TCP -Action Allow
New-NetFirewallRule -DisplayName "SQLServer Browser service" -Direction Inbound -LocalPort 1434 -Protocol UDP -Action Allow
```

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/en-us/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access?view=sql-server-ver15" rel="nofollow">https://docs.microsoft.com/en-us/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access?view=sql-server-ver15</a>
```

# <span class="mw-headline" id="bkmrk-benamte-instanz-1">Benamte Instanz</span>

Leider funktioniert es nicht bei benamten Instanzen. Hier ist der bessere Ansatz alles für einen bestimmten Server frei zu geben.  
[![SQL-Firewall-1.png](https://wiki.eidolf.de/images/thumb/4/4d/SQL-Firewall-1.png/700px-SQL-Firewall-1.png)](https://wiki.eidolf.de/index.php/Datei:SQL-Firewall-1.png)  
[![SQL-Firewall-2.png](https://wiki.eidolf.de/images/thumb/2/29/SQL-Firewall-2.png/700px-SQL-Firewall-2.png)](https://wiki.eidolf.de/index.php/Datei:SQL-Firewall-2.png)  
[![SQL-Firewall-3.png](https://wiki.eidolf.de/images/thumb/e/e7/SQL-Firewall-3.png/700px-SQL-Firewall-3.png)](https://wiki.eidolf.de/index.php/Datei:SQL-Firewall-3.png)  
[![SQL-Firewall-4.png](https://wiki.eidolf.de/images/thumb/9/9c/SQL-Firewall-4.png/700px-SQL-Firewall-4.png)](https://wiki.eidolf.de/index.php/Datei:SQL-Firewall-4.png)  
[![SQL-Firewall-5.png](https://wiki.eidolf.de/images/thumb/2/2e/SQL-Firewall-5.png/700px-SQL-Firewall-5.png)](https://wiki.eidolf.de/index.php/Datei:SQL-Firewall-5.png)  
[![SQL-Firewall-6.png](https://wiki.eidolf.de/images/2/28/SQL-Firewall-6.png)](https://wiki.eidolf.de/index.php/Datei:SQL-Firewall-6.png)  
[![SQL-Firewall-7.png](https://wiki.eidolf.de/images/thumb/f/fd/SQL-Firewall-7.png/700px-SQL-Firewall-7.png)](https://wiki.eidolf.de/index.php/Datei:SQL-Firewall-7.png)

## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://www.sqlserverlogexplorer.com/fix-error-code-26/" rel="nofollow">https://www.sqlserverlogexplorer.com/fix-error-code-26/</a>
```