---
title: wsus
description: 
published: true
date: 2023-12-31T13:35:50.407Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:35:47.194Z
---

# WSUS

# <span class="mw-headline" id="bkmrk-wsus-daten-verschieb-1">WSUS Daten verschieben</span>

Mit dem Befehl “wsusutil.exe movecontent” kann man die Daten bei einem WSUS 3.0 aufwärts verschieben.  
Das Tool ist unter “C:\\Programme\\Update Services\\Tools” zu finden.  
Wichtig ist das der Ziel Pfad komplett angelegt wird, sonst wird eine Fehlermeldung zurück gegeben.

## <span class="mw-headline" id="bkmrk-beispiel%3A-1">Beispiel:</span>

```
“C:\Programme\Update Services\Tools > wsusutil.exe movecontent d:\wsus d:\wsus\log.txt”
```

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://edvtraining.wordpress.com/2010/08/25/wsus-content-daten-verschieben/" rel="nofollow">http://edvtraining.wordpress.com/2010/08/25/wsus-content-daten-verschieben/</a>
```

# <span class="mw-headline" id="bkmrk-wsus-mit-sql-server-1">WSUS mit SQL Server</span>

Falls die Einrichtung von WSUS abbricht mit der Fehlermeldung  
`System.Data.SqlClient.SqlException (0x80131904): Login failed for user 'NT-AUTORITÄT\ANONYMOUS-ANMELDUNG'`  
Sollte folgende Lösung versucht werden.

<div class="vector-body" id="bkmrk-verbinden-auf-die-ko"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Verbinden auf die Kommandokonsole (CMD) des WSUS Server
2. Befehl ausführen `wsusutil.exe postinstall SQL_INSTANCE_NAME="SQLServer\Instanz" CONTENT_DIR=C:\WSUS`

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx?Sort=MostUseful&PageIndex=1" rel="nofollow">https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx?Sort=MostUseful&PageIndex=1</a>
```

# <span class="mw-headline" id="bkmrk-wsus-datenbank-berei-1">WSUS Datenbank bereinigen</span>

## <span class="mw-headline" id="bkmrk-benutzerdefinierte-i-1">Benutzerdefinierte Indizes erstellen</span>

Wird nur einmal benötigt um die Datenbereinigung Geschwindigkeit zu verbessern.  
**SQL Script:**

```
-- Create custom index in tbLocalizedPropertyForRevision
USE [SUSDB]

CREATE NONCLUSTERED INDEX [nclLocalizedPropertyID] ON [dbo].[tbLocalizedPropertyForRevision]
(
     [LocalizedPropertyID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]


-- Create custom index in tbRevisionSupersedesUpdate
CREATE NONCLUSTERED INDEX [nclSupercededUpdateID] ON [dbo].[tbRevisionSupersedesUpdate] 
( 
     [SupersededUpdateID] ASC 
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
```

## <span class="mw-headline" id="bkmrk-defragmentieren-der--1">Defragmentieren der Datenbank:</span>

**SQL Script:**

```
 /****************************************************************************** 
This sample T-SQL script performs basic maintenance tasks on SUSDB 
1. Identifies indexes that are fragmented and defragments them. For certain 
   tables, a fill-factor is set in order to improve insert performance. 
   Based on MSDN sample at http://msdn2.microsoft.com/en-us/library/ms188917.aspx 
   and tailored for SUSDB requirements 
2. Updates potentially out-of-date table statistics. 
******************************************************************************/ 
 
USE SUSDB; 
GO 
SET NOCOUNT ON; 
 
-- Rebuild or reorganize indexes based on their fragmentation levels 
DECLARE @work_to_do TABLE ( 
    objectid int 
    , indexid int 
    , pagedensity float 
    , fragmentation float 
    , numrows int 
) 
 
DECLARE @objectid int; 
DECLARE @indexid int; 
DECLARE @schemaname nvarchar(130);  
DECLARE @objectname nvarchar(130);  
DECLARE @indexname nvarchar(130);  
DECLARE @numrows int 
DECLARE @density float; 
DECLARE @fragmentation float; 
DECLARE @command nvarchar(4000);  
DECLARE @fillfactorset bit 
DECLARE @numpages int 
 
-- Select indexes that need to be defragmented based on the following 
-- * Page density is low 
-- * External fragmentation is high in relation to index size 
PRINT 'Estimating fragmentation: Begin. ' + convert(nvarchar, getdate(), 121)  
INSERT @work_to_do 
SELECT 
    f.object_id 
    , index_id 
    , avg_page_space_used_in_percent 
    , avg_fragmentation_in_percent 
    , record_count 
FROM  
    sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL , NULL, 'SAMPLED') AS f 
WHERE 
    (f.avg_page_space_used_in_percent < 85.0 and f.avg_page_space_used_in_percent/100.0 * page_count < page_count - 1) 
    or (f.page_count > 50 and f.avg_fragmentation_in_percent > 15.0) 
    or (f.page_count > 10 and f.avg_fragmentation_in_percent > 80.0) 
 
PRINT 'Number of indexes to rebuild: ' + cast(@@ROWCOUNT as nvarchar(20)) 
 
PRINT 'Estimating fragmentation: End. ' + convert(nvarchar, getdate(), 121) 
 
SELECT @numpages = sum(ps.used_page_count) 
FROM 
    @work_to_do AS fi 
    INNER JOIN sys.indexes AS i ON fi.objectid = i.object_id and fi.indexid = i.index_id 
    INNER JOIN sys.dm_db_partition_stats AS ps on i.object_id = ps.object_id and i.index_id = ps.index_id 
 
-- Declare the cursor for the list of indexes to be processed. 
DECLARE curIndexes CURSOR FOR SELECT * FROM @work_to_do 
 
-- Open the cursor. 
OPEN curIndexes 
 
-- Loop through the indexes 
WHILE (1=1) 
BEGIN 
    FETCH NEXT FROM curIndexes 
    INTO @objectid, @indexid, @density, @fragmentation, @numrows; 
    IF @@FETCH_STATUS < 0 BREAK; 
 
    SELECT  
        @objectname = QUOTENAME(o.name) 
        , @schemaname = QUOTENAME(s.name) 
    FROM  
        sys.objects AS o 
        INNER JOIN sys.schemas as s ON s.schema_id = o.schema_id 
    WHERE  
        o.object_id = @objectid; 
 
    SELECT  
        @indexname = QUOTENAME(name) 
        , @fillfactorset = CASE fill_factor WHEN 0 THEN 0 ELSE 1 END 
    FROM  
        sys.indexes 
    WHERE 
        object_id = @objectid AND index_id = @indexid; 
 
    IF ((@density BETWEEN 75.0 AND 85.0) AND @fillfactorset = 1) OR (@fragmentation < 30.0) 
        SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REORGANIZE'; 
    ELSE IF @numrows >= 5000 AND @fillfactorset = 0 
        SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REBUILD WITH (FILLFACTOR = 90)'; 
    ELSE 
        SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REBUILD'; 
    PRINT convert(nvarchar, getdate(), 121) + N' Executing: ' + @command; 
    EXEC (@command); 
    PRINT convert(nvarchar, getdate(), 121) + N' Done.'; 
END 
 
-- Close and deallocate the cursor. 
CLOSE curIndexes; 
DEALLOCATE curIndexes; 
 
 
IF EXISTS (SELECT * FROM @work_to_do) 
BEGIN 
    PRINT 'Estimated number of pages in fragmented indexes: ' + cast(@numpages as nvarchar(20)) 
    SELECT @numpages = @numpages - sum(ps.used_page_count) 
    FROM 
        @work_to_do AS fi 
        INNER JOIN sys.indexes AS i ON fi.objectid = i.object_id and fi.indexid = i.index_id 
        INNER JOIN sys.dm_db_partition_stats AS ps on i.object_id = ps.object_id and i.index_id = ps.index_id 
 
    PRINT 'Estimated number of pages freed: ' + cast(@numpages as nvarchar(20)) 
END 
GO 
 
 
--Update all statistics 
PRINT 'Updating all statistics.' + convert(nvarchar, getdate(), 121)  
EXEC sp_updatestats 
PRINT 'Done updating statistics.' + convert(nvarchar, getdate(), 121)  
GO 
```

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-veraltete-updates-l%C3%B6-1">Veraltete Updates löschen</span>

Veraltete Updates ablehnen:

```
Get-WsusUpdate -Classification All -Status Any -Approval AnyExceptDeclined | Where-Object { $_.Update.GetRelatedUpdates(([Microsoft.UpdateServices.Administration.UpdateRelationship]::UpdatesThatSupersedeThisUpdate)).Count -gt 0} | Deny-WsusUpdate
```

Danach die Updates bereinigen:

```
Invoke-WsusServerCleanup -CleanupObsoleteUpdates -CleanupUnneededContentFiles
```

### <span class="mw-headline" id="bkmrk-cleanup-script-1">Cleanup Script</span>

Nach verschiedenen Lösungsansätzen die ausprobiert wurden komme ich zum Schluß das folgendes Script eines der besten ist um den WSUS zu bereinigen.

<div class="vector-body" id="bkmrk-https%3A%2F%2Fgithub.com%2Fd"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- [https://github.com/djdomi/WSUS-Cleanup-Scripts/blob/master/wsus-cleanup-updates-v4.ps1](https://github.com/djdomi/WSUS-Cleanup-Scripts/blob/master/wsus-cleanup-updates-v4.ps1)

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://support.microsoft.com/de-de/help/4490644/complete-guide-to-microsoft-wsus-and-configuration-manager-sup-maint" rel="nofollow">https://support.microsoft.com/de-de/help/4490644/complete-guide-to-microsoft-wsus-and-configuration-manager-sup-maint</a>
<a class="external free" href="https://dscottraynsford.wordpress.com/2015/05/13/wsus-declining-all-superceded-updates/" rel="nofollow">https://dscottraynsford.wordpress.com/2015/05/13/wsus-declining-all-superceded-updates/</a>
```

# <span class="mw-headline" id="bkmrk-wsus-auf-ssl-umstell-1">WSUS auf SSL umstellen</span>

<div class="vector-body" id="bkmrk-webserverzertifikat-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Webserverzertifikat für WSUS FQDN ausstellen und im local machine store hinterlegen
2. Am IIS den Port 8531 an das Zertifikat binden.
3. Jeweils für folgende Unterseiten der **WSUS-Verwaltung**, **SSL erforderlich** aktivieren. 
    - APIremoting30
    - ClientWebService
    - DSSAuthWebService
    - ServerSyncWebService
    - SimpleAuthWebService
4. Eine CMD Konsole öffnen und in folgenden Pfad wechseln `%ProgramFiles%\Update Services\Tools\`
5. Folgenden Befehl verwenden um auf SSL umzustellen `wsusutil.exe configuressl FQDN-des-Servers`

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-7">Quelle:</span>

```
<a class="external free" href="https://www.windowspro.de/wolfgang-sommergut/wsus-fuer-ssl-verbindung-konfigurieren" rel="nofollow">https://www.windowspro.de/wolfgang-sommergut/wsus-fuer-ssl-verbindung-konfigurieren</a>
```

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span class="mw-headline" id="bkmrk-verbindung-zu-wsus-b-1">Verbindung zu WSUS bricht ab</span>

Das kann viele Ursachen haben doch wenn folgender Fehler in der Ereignisanzeige steht **WAS 5002**, ist der Anwendungspool am WebServer abgestürzt.

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

<div class="vector-body" id="bkmrk-iis-manager-%C3%B6ffnen-a"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. IIS Manager öffnen
2. Anwendungspools auswählen
3. WSUS Pool &gt; Erweiterte Eigenschaften
4. Folgende Werte Eintragen 
    1. Warteschlangenlänge: **25000** | <small>Alter Wert: 3000</small>
    2. Limit für den privaten Speicher: 0 | <small>Alter Wert: 7843200</small>

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-9">Quelle:</span>

```
<a class="external free" href="https://blog.blackseals.net/2017/07/28/fehler-bei-suche-nach-updates-0x8024401c/" rel="nofollow">https://blog.blackseals.net/2017/07/28/fehler-bei-suche-nach-updates-0x8024401c/</a>
<a class="external free" href="https://jmcblog.de/2017/03/24/loesung-microsoft-windows-was-5002-wsus-webserver-stuerzt-ab/" rel="nofollow">https://jmcblog.de/2017/03/24/loesung-microsoft-windows-was-5002-wsus-webserver-stuerzt-ab/</a>
```

## <span class="mw-headline" id="bkmrk-wsus-kann-nicht-mehr-1">WSUS kann nicht mehr synchronisieren</span>

Folgendes konnte ich bei einem WSUS an einem Server 2016 feststellen (Die gefundene Lösung war bei einer älteren Version, also Schätzungsweise MS Produktqualität seit Jahren schlecht)

<div class="vector-body" id="bkmrk-an-clients-sieht-man"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- An Clients sieht man keine Fehlermeldung, nur das Updates schon seit einem Monat nicht installiert wurden.
- Am WSUS kann nicht synchronisiert werden
- Am WSUS kann man keine Updates freigeben oder abweisen (Fehlermeldung erscheint)
- Letzte Synchronisierung hat den Status "Unbekannt"
- Bereinigung der Datenbank funktioniert nicht über die GUI
- Neustart der Dienst oder des Server bringt keine Besserung

</div></div></div>### <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-3">Lösung:</span>

Wohlgemerkt habe ich keinen SSL WSUS und dachte eigentlich nicht das die Lösung zum Erfolg führt.

<div class="vector-body" id="bkmrk-powershell-%C3%B6ffnen-in"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Powershell öffnen
2. In den Ordner `C:\Programme\Update Services\Tools` wechseln
3. Befehle eingeben ```
    .\wsusutil.exe configuressl voller.wsus.fqdn
    Restart-Service wsusservice
    Restart-Service bits
    iisreset
    ```

</div></div></div>#### <span class="mw-headline" id="bkmrk-quelle%3A-11">Quelle:</span>

```
<a class="external free" href="https://social.technet.microsoft.com/Forums/de-DE/bd5b1b2f-7db6-47fa-89a1-5bdda111c5b4/wsus-server-synchronisiert-nach-windows-update-nicht-mehr?forum=windows_Serverde" rel="nofollow">https://social.technet.microsoft.com/Forums/de-DE/bd5b1b2f-7db6-47fa-89a1-5bdda111c5b4/wsus-server-synchronisiert-nach-windows-update-nicht-mehr?forum=windows_Serverde</a>
<a class="external free" href="https://www.wsus.de/cgi-bin/yabb/YaBB.pl?num=1339526094" rel="nofollow">https://www.wsus.de/cgi-bin/yabb/YaBB.pl?num=1339526094</a>
```

## <span class="mw-headline" id="bkmrk-wsus-kann-datenbank--1">WSUS kann Datenbank nicht bereinigen</span>

Da meine Lösung zum Synchronisierungsproblem zwar halbwegs Besserung gebracht hat aber leider nicht beim bereinigen der Datenbank hier die nächste Lösung.  
Bereinigung der Datenbank über SQL Abfrage.

### <span id="bkmrk--3"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung-1%3A-1">Lösung 1:</span>

Ich habe untenstehende Lösung 2 als erstes ausprobiert, doch auch hier kam es zu Problemen beim Löschen der unnötigen Updates. Nach einer weiteren Suche gab es folgenden Hinweis.

<div class="vector-body" id="bkmrk-zur-wsus-sql-datenba"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Zur WSUS SQL Datenbank verbinden
2. Eigenschaften der Datenbank aufrufen (Rechtsklick auf den obersten Punkt der Baumstruktur)
3. Reiter Verbindungen
4. Timeout für Remoteabfragen von 600 auf 0 (Unendlich) setzen
5. Mit **OK** speichern

</div></div></div>Nach dieser Änderung konnte ich sogar die normal Bereinigung über Powershell durchführen  
`Get-WsusServer | Invoke-WsusServerCleanup -CleanupObsoleteUpdates -CleanupUnneededContentFiles -CompressUpdates -DeclineExpiredUpdates -DeclineSupersededUpdates`

#### <span class="mw-headline" id="bkmrk-quelle%3A-13">Quelle:</span>

```
<a class="external free" href="https://social.technet.microsoft.com/Forums/sharepoint/en-US/7b12f8b2-d0e6-4f63-a98a-019356183c29/getting-past-wsus-cleanup-wizard-time-out-removing-unnecessary-updates?forum=winserverwsus" rel="nofollow">https://social.technet.microsoft.com/Forums/sharepoint/en-US/7b12f8b2-d0e6-4f63-a98a-019356183c29/getting-past-wsus-cleanup-wizard-time-out-removing-unnecessary-updates?forum=winserverwsus</a>
```

### <span id="bkmrk--4"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung-2%3A-1">Lösung 2:</span>

<div class="vector-body" id="bkmrk-entweder-auf-die-lok"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Entweder auf die lokale WSUS Datenbank verbinden oder wie in meinem Fall auf die SQL Datenbank des WSUS
2. Folgendes Script ausführen:

</div></div></div>```
USE SUSDB;
DECLARE @var1 INT 
DECLARE @msg nvarchar(100) 
CREATE TABLE #results (Col1 INT) INSERT INTO #results(Col1) 
EXEC spGetObsoleteUpdatesToCleanup 
DECLARE WC Cursor FOR SELECT Col1 FROM #results 
OPEN WC 
FETCH NEXT FROM WC INTO @var1 WHILE (@@FETCH_STATUS > -1) 
BEGIN SET @msg = 'Deleting ' + CONVERT(varchar(10), @var1) RAISERROR(@msg,0,1) WITH NOWAIT 
EXEC spDeleteUpdate @localUpdateID=@var1 
FETCH NEXT FROM WC INTO @var1 
END 
CLOSE WC 
DEALLOCATE WC 
DROP TABLE #results
```

#### <span class="mw-headline" id="bkmrk-quelle%3A-15">Quelle:</span>

```
<a class="external free" href="https://www.administrator.de/wissen/wsus-assistent-serverbereinigung-bricht-l%C3%B6schen-nicht-ben%C3%B6tigter-updates-serverknoten-zur%C3%BCcksetzen-330038.html" rel="nofollow">https://www.administrator.de/wissen/wsus-assistent-serverbereinigung-bricht-l%C3%B6schen-nicht-ben%C3%B6tigter-updates-serverknoten-zur%C3%BCcksetzen-330038.html</a>
```