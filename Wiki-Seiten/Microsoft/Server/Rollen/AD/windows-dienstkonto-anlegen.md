# Windows Dienstkonto anlegen

## <span class="mw-headline" id="bkmrk-managed-service-acco-1">Managed Service Accounts</span>

Die ab 2008 R2 eingeführten Managed Service Accounts werden direkt vom System verwaltet, bedeutet man muss sich nicht um eine Passwortänderung kümmern und dadurch wird die Sicherheit erhöht.

### <span class="mw-headline" id="bkmrk-voraussetzung-1">Voraussetzung</span>

<div class="vector-body" id="bkmrk-ad-einheitlich-auf-s"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- AD einheitlich auf Server 2008R2 oder höher, sonst eine Schemaerweiterung durchführen
- Powershell

</div></div></div>### <span class="mw-headline" id="bkmrk-erstellung-1">Erstellung</span>

Powershell als Administrator öffnen  
Active Directory Modul importieren

```
import-Module ActiveDirectory
```

Account anlegen

```
New-ADServiceAccount -SamAccountName MyService -Name MyService
```

Account im AD installieren/initialisieren

```
Install-ADServiceAccount -Identity MyService
```

### <span class="mw-headline" id="bkmrk-quelle-1">Quelle</span>

```
<a class="external free" href="http://blog.icewolf.ch/archive/2010/08/03/managed-service-accounts-in-windows-2008-r2.aspx" rel="nofollow">http://blog.icewolf.ch/archive/2010/08/03/managed-service-accounts-in-windows-2008-r2.aspx</a>
```