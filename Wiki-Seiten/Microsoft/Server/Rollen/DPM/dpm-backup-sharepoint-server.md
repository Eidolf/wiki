# DPM Backup Sharepoint Server

Wenn Änderungen am Sharepoint vorgenommen wurden kann es sein, das vorhandene Backupaufträge fehlschlagen. Es werden vom DPM vorschläge gemacht das man den " WSSCmdletsWrapper " neu registriert.

<div class="vector-body" id="bkmrk-mit-folgender-vorgeh"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Mit folgender Vorgehensweise kann man dies reparieren:

1. Herausfinden welcher Benutzer als Farm Administrator eingetragen ist 
    1. Befehl ausführen **dcomcnfg**, oder die Komponentendienste über den Explorer öffnen
    2. Unter **Component Services &gt; Computers &gt; My Computers &gt; DCOM Config** nach dem **WSSCmdletWrapper** Objekt suchen.
    3. Die Eigenschaften über einen Rechtsklick aufrufen
    4. Auf dem Identitäten Reiter kann man überprüfen welcher Benutzer eingetragen ist, dieser wird für die nachfolgenden Befehle benötigt.
2. Am Sharepoint Server folgenden Befehl ausführen ```
    C:\Program Files\Microsoft Data Protection Manager\DPM\bin>ConfigureSharepoint.exe -EnableSharepointProtection
    ```
3. Wenn zusätzlich der Search Server installiert ist, dann noch folgenden Befehl eingeben ```
    C:\Program Files\Microsoft Data Protection Manager\DPM\bin>ConfigureSharepoint.exe -EnableSPSearchProtection
    ```

- Wenn einer der zwei oberen Punkte fehlschlagen dann sollte man zur Fehlerbehebung folgendermaßen vorgehen:

1. Go to "%commonprogramfiles%\\Microsoft Shared\\web server extensions\\12\\BIN", and run "stsadm -o registerwsswriter" command.
2. If the user with which you installed dpm agent is not an admin, then give full permissions to the user on: 
    1. DPM folder (%programfiles%\\Microsoft Data Protection Manager)
    2. Registry Key (HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Microsoft Data Protection Manager)
3. Change wsscmdletswrapper dcom class's identity to the farm admin 
    1. From the command prompt, run dcomcnfg.
    2. Under Component Services-&gt;Computers-&gt;My Computers-&gt;DCOM Config, search for the WSSCmdletWrapper object.
    3. Right-click the object and select Properties.
    4. On the Identity tab, enter the credentials of the farm admin.
4. Go to directory "%windir%\\Microsoft.NET\\Framework\\v2.0.50727", and run Manager\\DPM\\bin\\WSSCmdlets.dll". (For WSS4/O14, register WSS4Cmdlets.dll instead)

</div></div></div>Quelle:

```
<a class="external free" href="http://www.eggheadcafe.com/software/aspnet/35386296/problems-backup-sharepoint-services-30-with-dpm2010.aspx" rel="nofollow">http://www.eggheadcafe.com/software/aspnet/35386296/problems-backup-sharepoint-services-30-with-dpm2010.aspx</a>
```