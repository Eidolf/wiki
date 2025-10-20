---
title: ad-connect
description: 
published: true
date: 2023-12-31T13:31:25.180Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:22.132Z
---

# AD Connect

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span class="mw-headline" id="bkmrk-indexoutofrangeexcep-1">IndexOutOfRangeException</span>

Bei der Installation von AAD Connect Version 1.1.819 kam es zu folgendem Fehler.  
[![IndexOutofRange.png](https://wiki.eidolf.de/images/thumb/d/d0/IndexOutofRange.png/700px-IndexOutofRange.png)](https://wiki.eidolf.de/index.php/Datei:IndexOutofRange.png)  
Deutsch: **Der Index lag außerhalb des Bereichs. Er darf nicht negativ und kleiner als die Auflistung sein.**  
Der Server wurde davor frisch installiert, da ich davor mit Altlasten in der Registry Probleme hatte.

### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

Die Lösung habe ich aus der Quelle, bei der ein Updatevorgang zum gleichen Problem geführt hat.  
Unter dem Pfad: **C:\\ProgramData\\AADConnect**, die **PersistedState.xml** finden.  
Auf die Datei hatte ich keine Schreibrechte, deshalb musste ich diese erst meinem verwendeten Administrator gewähren.  
**Der AAD Connect Wizard muss für das weitere Vorgehen geschlossen sein!**  
Anschließend die Datei mit dem Editor öffnen und folgendes einfügen bzw. ersetzen. (lokale.domäne ersetzen mit der eigenen)

```
    <PersistedStateElement>
      <Key>IAdfsContext.TargetForestDomainName</Key>
      <Value>lokale.domäne,lokale.domäne</Value>
    </PersistedStateElement>
```

Meine Datei hat zum Schluss folgend ausgesehen

```
<?xml version="1.0" encoding="utf-8"?>
<PersistedStateContainer xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <StateVersion>2</StateVersion>
  <Elements>
    <PersistedStateElement>
      <Key>IAdfsContext.TargetForestDomainName</Key>
      <Value>lokale.domäne,lokale.domäne</Value>
    </PersistedStateElement>
    <PersistedStateElement>
      <Key>IAzureActiveDirectoryContext.TenantId</Key>
      <Value>{00000000-0000-0000-0000-000000000000}</Value>
    </PersistedStateElement>
  </Elements>
</PersistedStateContainer>
```

Es kann natürlich auch sein, das alleine schon das berechtigen des Administrators ausreichend war, doch funktionierte es mit der obengenannten Version ganz sicher.

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://social.msdn.microsoft.com/Forums/en-US/6b94b062-9888-4c15-b9ed-5a9c71c718d7/error-after-update-111050-azure-ad-connect?forum=WindowsAzureAD" rel="nofollow">https://social.msdn.microsoft.com/Forums/en-US/6b94b062-9888-4c15-b9ed-5a9c71c718d7/error-after-update-111050-azure-ad-connect?forum=WindowsAzureAD</a>
```