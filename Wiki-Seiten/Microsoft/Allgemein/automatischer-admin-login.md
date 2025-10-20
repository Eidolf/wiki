---
title: automatischer-admin-login
description: 
published: true
date: 2025-06-27T16:06:04.454Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:28:25.658Z
---

# Automatischer Admin Login

Um einen automatischen Login zu vollziehen, der nur Sinn macht wenn ein einziger Benutzer (Administrator) an diesem Rechner vorhanden ist muss man in der Registry unter

- Hkey\_lokal\_machine -> Software -> Microsoft -> Windows NT -> CurrentVersion -> winlogon

den Schlüssel **Admin Auto Login** auf **1** stellen.

oder

- Start > Ausführen > **control userpasswords2** eingeben

hiermit öffnet sich die Benuzterverwaltung. In der Benutzerverwaltung den Haken **Benutzer müssen Benutzernamen und Kennwort eingeben** entfernen. Dadurch öffnet das Fenster Automatische Anmeldung, dort kann der entsprechende Benutzername mit Kennwort eingetragen werden der automatisch beim hochfahren angemeldet werden soll.

Quelle:

http://www.administrator.de/index.php?content=27545