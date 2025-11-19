---
title: AD Anheben des Domain Function Levels
description: 
published: true
date: 2025-11-19T16:49:54.341Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:03.977Z
---

# Beschreibung

Das Forest und Domain Level sollte bestmöglich auf dem höchsten Stand gehalten werden um alle Funktionen nutzen zu können.

# Hinweis

Falls zuvor das Forest Level angehoben wurde kann das Domain Function Level nicht niedriger sein und muss somit gleichzeit angehoben worden.
![Forest-005.png](/media/Forest-005.png)
> Besser gesagt, erst das Domain Function Level anheben und danach das Forest Function level wenn alle Domains auf der gewünschten Version sind
{.is-warning}


# Durchführung

1. Active Directory Domains and Trusts (Domänen und Vertrauenstellung) öffnen 
![forest-001.png](/media/forest-001.png)
2. Auf die Domäne mit einem Rechtsklick klicken > Raise Domain Function Level 
![forest-004.png](/media/forest-004.png)
3. Das gewünschte Level auswählen und mit OK bestätigen
![forest-006.png](/media/forest-006.png)
4. Eine Warnung bestätigen und hoffentlich folgendes Fenster sehen
![forest-007.png](/media/forest-007.png)

## Wer es ohne GUI machen möchte
`Set-ADDomainMode -Identity <DomainName> -DomainMode Windows2016Domain`

# Nacharbeiten
Ich führe folgende Befehle nach jeder Domäne aus und warte zusätzlich eine Zeit ab.

1. Prüfen des Domain Level mit PowerShell `Get-ADDomain | Select DomainMode`
2. Synchronisierung anstoßen > `repadmin /syncall /AdeP`
3. Prüfen der Replizierung > `repadmin /showrepl`
4. Prüfen des ***"Directory Service Logs"***
5. Allgemeine Funktionsprüfung: ***Anmeldung, Kerberos, GPOs, DNS.***