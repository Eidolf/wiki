---
title: Github
description: Git Befehle für die Verwaltung von Github
published: true
date: 2026-04-17T12:51:10.238Z
tags: git, development
editor: markdown
dateCreated: 2026-04-15T00:57:18.294Z
---

# Konfiguration
## Auslesen
`git config --global user.name` | Benutzername auslesen
`git config --global user.email` | Benutzer E-Mail auslesen
## Setzen
### Benutzerdaten
Um Commits mit der richtigen Person zu erstellen sollte man die Globale Konfiguration anpassen.
`git config --global user.name "Your Name"` | Setzen des Github Benutzernamens
`git config --global user.email "your_email@example.com"` | Setzen der Github Benutzer E-Mail Adresse

# Standard Aufgaben
## Pull (Daten von github ziehen)
`git pull`

### Rebase
Wenn der lokale Stand noch nicht aktualisiert wurde mit neueren Daten auf github, man dennoch Änderungen lokal durchgeführt hat, kann man keinen normalen Pull oder Sync durchführen.
Hier müssen die Cloud Daten neu einsortiert werden, das geht mit **--rebase**
`git pull --rebase`

## Push (Daten nach github schieben)
`git push`

# Erweiterte Aufgaben
## Autor des letzten Commits ändern
Bei Commits kann es passieren das ausversehen ein falscher Name in den Einstellungen hinterlegt ist und man diesen falschen Author zurückändern möchte, da ansonsten die Commit History unsauber aussieht.
`git commit --amend --reset-author`
Nach diesem Befehl muss ein Erzwungener Push durchgeführt werden
`git push -f`

## Git History bereinigen

1. Checkout/Erstellen eines Orphan-Branches (dieser Branch erscheint nicht im git branch‑Befehl):
`git checkout --orphan latest_branch`

2. Alle Dateien zum neu erstellten Branch hinzufügen:
`git add -A`

3. Änderungen committen:
`git commit -am "commit message"`

4. Hauptbranch (Standardbranch) löschen (dieser Schritt ist dauerhaft):
`git branch -D main`

5. Aktuellen Branch in main umbenennen:
`git branch -m main`

6. Abschließend: Alle Änderungen sind lokal abgeschlossen, jetzt das Remote‑Repository zwangsweise aktualisieren:
`git push -f origin main`
### Quelle
https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github