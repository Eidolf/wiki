---
title: Github
description: Git Befehle für die Verwaltung von Github
published: true
date: 2026-04-15T00:57:18.295Z
tags: git, development
editor: markdown
dateCreated: 2026-04-15T00:57:18.294Z
---

# Standard Aufgaben
## Pull (Daten von github ziehen)
`git pull`
# Erweiterte Aufgaben
## Autor des letzten Commits ändern
Bei Commits kann es passieren das ausversehen ein falscher Name in den Einstellungen hinterlegt ist und man diesen falschen Author zurückändern möchte, da ansonsten die Commit History unsauber aussieht.
`git commit --amend --reset-author`
Nach diesem Befehl muss ein Erzwungener Push durchgeführt werden
`git push -f`