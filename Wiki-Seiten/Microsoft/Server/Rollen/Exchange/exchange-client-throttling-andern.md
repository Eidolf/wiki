---
title: exchange-client-throttling-andern
description: 
published: true
date: 2023-12-31T14:33:18.272Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:33:15.109Z
---

# Exchange Client Throttling ändern

# <span class="mw-headline" id="bkmrk-anzeigen-der-policys-1">Anzeigen der Policys</span>

## <span class="mw-headline" id="bkmrk-anzeigen-aller-benut-1">Anzeigen aller Benutzer und deren momentane Throttling Policy</span>

```
Get-Mailbox | fl throttlingpolicy, identity
```

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-anzeigen-der-throttl-1">Anzeigen der Throttling Policy für einen bestimmten Benutzer</span>

```
Get-Mailbox -identity username | fl throttlingpolicy
```

# <span class="mw-headline" id="bkmrk-bearbeiten-von-polic-1">Bearbeiten von Policys</span>

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-hinzuf%C3%BCgen-einer-neu-1">Hinzufügen einer neuen Throttling Policy</span>

```
New-ThrottlingPolicy -Name ClientThrottlingPolicy2 -Option1 80 -Option2 $null;
```

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-l%C3%B6schen-einer-thrott-1">Löschen einer Throttling Policy</span>

```
Remove-ThrottlingPolicy ClientThrttlingPolicy2
```

## <span class="mw-headline" id="bkmrk-neue-throttling-poli-1">Neue Throttling Policy einem Benutzer zuordnen</span>

```
$b = Get-ThrottlingPolicy ClientThrottlingPolicy2;
Set-Mailbox -Identity tonysmith -ThrottlingPolicy $b;
```

## <span class="mw-headline" id="bkmrk-standard-throttling--1">Standard Throttling Policy einem Benutzer zuordnen</span>

```
$policy = Get-ThrottlingPolicy | where-object {$_.IsDefault -eq $true}
Set-Mailbox -Identity tonysmith -ThrottlingPolicy $policy;
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/dd298094.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/dd298094.aspx</a>
<a class="external free" href="http://technet.microsoft.com/en-us/library/dd297964.aspx" rel="nofollow">http://technet.microsoft.com/en-us/library/dd297964.aspx</a>
<a class="external free" href="http://de.help.mailstore.com/E-Mail-Archivierung_von_Microsoft_Exchange_2010#Throttling_in_Exchange_2010_SP1" rel="nofollow">http://de.help.mailstore.com/E-Mail-Archivierung_von_Microsoft_Exchange_2010#Throttling_in_Exchange_2010_SP1</a>
```