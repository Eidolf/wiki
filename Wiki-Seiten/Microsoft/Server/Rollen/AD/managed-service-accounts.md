---
title: managed-service-accounts
description: 
published: true
date: 2023-12-31T13:31:20.698Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:17.648Z
---

# Managed Service Accounts

# <span class="mw-headline" id="bkmrk-managed-service-acco-1">Managed Service Accounts</span>

## <span class="mw-headline" id="bkmrk-erstellen-1">Erstellen</span>

```
New-ADServiceAccount -Name svc-test -DNSHostName svc-test.domaene.local -PrincipalsAllowedToRetrieveManagedPassword Zielserver$ oder Array von Servern getrennt mit Komma
```

## <span class="mw-headline" id="bkmrk-verwenden-1">Verwenden</span>

AD Powershell mit folgendem Befehl installieren

```
DISM /online /enable-feature /all /featurename=ActiveDirectory-PowerShell
```

Auf dem Zielserver folgendes mit einer AD Powershell eingeben um den Account zu verwenden

```
Install-ADServiceAccount svc-test
```

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-%C3%84ndern-1">Ändern</span>

Account für neuen Server freigeben:

```
Set-ADServiceAccount svc-test -PrincipalsAllowedToRetrieveManagedPassword VorhandenerServer$,NeuerServer$
```

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://blogs.msdn.microsoft.com/arvindsh/2014/02/03/managed-service-accounts-msa-and-sql-2012-practical-tips/" rel="nofollow">https://blogs.msdn.microsoft.com/arvindsh/2014/02/03/managed-service-accounts-msa-and-sql-2012-practical-tips/</a>
<a class="external free" href="https://social.technet.microsoft.com/Forums/ie/de-DE/7519b2ec-67f0-40ac-88a6-69ed7b47a34b/installadserviceaccount-access-denied?forum=active_directoryde" rel="nofollow">https://social.technet.microsoft.com/Forums/ie/de-DE/7519b2ec-67f0-40ac-88a6-69ed7b47a34b/installadserviceaccount-access-denied?forum=active_directoryde</a>
<a class="external free" href="https://technet.microsoft.com/de-de/library/jj128431(v=ws.11).aspx" rel="nofollow">https://technet.microsoft.com/de-de/library/jj128431(v=ws.11).aspx</a>
```