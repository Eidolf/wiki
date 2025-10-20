---
title: powershell-abfragen
description: 
published: true
date: 2023-12-31T13:29:06.961Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:29:03.873Z
---

# PowerShell Abfragen

# <span class="mw-headline" id="bkmrk-ad%3A-1">AD:</span>

Ein Benutzer gehört nicht der primären Gruppe der " Domänenbenutzer " an  
Folgende Abfrage für die Powershell benötigt das die [AD CMDlets](http://www.quest.com/powershell/activeroles-server.aspx%7CQuest) installiert sind.

```
 # PrimaryGroupID for 'Domain Users' = 513
$users = Get-QADUser
foreach ($user in $users) 
{
    if ($user.PrimaryGroupId -ne 513) 
    {
        Write-Host $user.DisplayName " : " $user.DN
    }
}
```

Das gleiche in Kurzform

```
# As a one-liner
Get-QADUser | %{if($_.PrimaryGroupId -ne 513){$_.DN}}
```

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://grinding-it-out.blogspot.com/2008/09/memberof-does-not-list-primary-group.html" rel="nofollow">http://grinding-it-out.blogspot.com/2008/09/memberof-does-not-list-primary-group.html</a>
```

# <span class="mw-headline" id="bkmrk-exchange">Exchange</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Abfragen&action=edit&section=3 "Abschnitt bearbeiten: Exchange")<span class="mw-editsection-bracket">\]</span></span>

Folgende Abfragen werden über die Exchange Powershell ausgeführt.

  
**Alle E-Mail Adressen einer Organisation sortiert nach Benutzern, geschrieben in mail.txt Datei**

```
get-mailbox |fl displayname, emailaddresses > C:\Pfad\mail.txt
```

**Alle Benutzerpostfächer die Postmaster in einer Ihrer Absendeadressen beinhalten**

```
get-mailbox |where-object {$_.EmailAddresses -like '*postmaster*'}
```