---
title: Systemvariablen
description: Informationen zu den Windows Systemvariablen
published: true
date: 2026-01-16T15:46:37.741Z
tags: windows client, windows server, systemvariablen, system variables
editor: markdown
dateCreated: 2026-01-16T15:45:09.811Z
---


# Anleitung: Auslesen und Setzen von Systemvariablen über PowerShell

Im Folgenden wird gezeigt, wie man Systemvariablen in PowerShell ausliest und setzt. Dies ist besonders nützlich, um den **PATH** oder andere Umgebungsvariablen für den Benutzer oder das gesamte System zu ändern.

## Aktuellen Wert einer Umgebungsvariablen auslesen

Um den aktuellen Wert einer Umgebungsvariablen (z. B. `Path`) auszulesen, kann folgender Befehl verwendet werden:

```powershell
[System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::Machine)
```

- **Machine**: Liest die Variable für das gesamte System.
- **User**: Liest die Variable für den aktuellen Benutzer.
- **Process**: Liest die Variable für die aktuelle Sitzung.

Beispiel für den Benutzer:

```powershell
[System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::User)
```

## Neue Werte setzen

Um den Wert einer Umgebungsvariablen zu setzen, wird folgender Befehl genutzt:

```powershell
$newPath = "C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\"

[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::Machine)
```

### Für den Benutzer:

```powershell
[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::User)
```

### Für den aktuellen Prozess:

```powershell
[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::Process)
```

## Hinweis

- Änderungen für **Machine** erfordern Administratorrechte.
- Nach dem Setzen einer Systemvariablen muss die Sitzung ggf. neu gestartet werden, damit die Änderungen wirksam werden.

---
> **Tipp:** Wenn du den bestehenden Wert erweitern möchtest, kannst du den aktuellen Wert auslesen und an den neuen Wert anhängen:
{.is-info}


```powershell
$currentPath = [System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::Machine)
$newPath = $currentPath + ";C:\Neuer\Pfad"
[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::Machine)
```
