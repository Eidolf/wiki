---
title: sql-2008-r2-systemdatenbanken-verschieben
description: 
published: true
date: 2023-12-31T13:34:13.162Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:34:09.052Z
---

# SQL 2008 R2 Systemdatenbanken verschieben

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-model%2C-msdb%2C-tempdb--1">model, msdb, tempdb Datenbanken</span>

Diese Anleitung ist für die Systemdatenbanken ( ***model, msdb, tempdb*** ) gedacht, **nicht** für die ***master*** und ***Reporting*** Datenbanken.

<div class="vector-body" id="bkmrk-ermitteln-sie-die-lo"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Ermitteln Sie die logischen Dateinamen der tempdb-Datenbank und ihren aktuellen Speicherort auf dem Datenträger. ```
    SELECT name, physical_name AS CurrentLocation
    FROM sys.master_files
    WHERE database_id = DB_ID(N'tempdb');
    GO
    ```
2. Ändern Sie den Speicherort der einzelnen Dateien mithilfe von ALTER DATABASE. ```
    USE master;
    GO
    ALTER DATABASE tempdb 
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');
    GO
    ALTER DATABASE tempdb 
    MODIFY FILE (NAME = templog, FILENAME = 'F:\SQLLog\templog.ldf');
    GO
    ```
3. Beenden Sie die Instanz von SQL Server
4. Verschieben der Datenbanken an den neuen Dateipfad (tempdb muss nicht verschoben werden, da diese beim starten des SQL Dienstes neu erstellt wird)
5. Starten der SQL Server Instanz
6. Überprüfen Sie die Dateiänderung. ```
    SELECT name, physical_name AS CurrentLocation, state_desc
    FROM sys.master_files
    WHERE database_id = DB_ID(N'tempdb');
    ```
7. Löschen Sie die Dateien tempdb.mdf und templog.ldf vom ursprünglichen Speicherort.

</div></div></div># <span class="mw-headline" id="bkmrk-master-datenbank-1">master Datenbank</span>

<div class="vector-body" id="bkmrk-zeigen-sie-im-men%C3%BC-s"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Zeigen Sie im Menü Start auf Alle Programme, auf Microsoft SQL Server 2005, auf Konfigurationstools, und klicken Sie dann auf SQL Server-Konfigurations-Manager.
2. Klicken Sie im Knoten SQL Server-Dienste mit der rechten Maustaste auf die Instanz von SQL Server (z. B. SQL Server (MSSQLSERVER)), und wählen Sie Eigenschaften aus.
3. Klicken Sie im Dialogfeld Eigenschaften von SQL Server (instance\_name) auf die Registerkarte Erweitert.
4. Bearbeiten Sie die Werte unter Startparameter so, dass sie auf den geplanten Speicherort für die Daten- und Protokolldateien der Masterdatenbank zeigen, und klicken Sie auf OK. Das Verschieben der Fehlerprotokolldatei ist optional. Der Parameterwert der Datendatei muss dem -d-Parameter und der Wert der Protokolldatei muss dem -l-Parameter entsprechen. Im folgenden Beispiel werden die Parameterwerte für den Standardspeicherort der master-Daten- und Protokolldateien dargestellt. ```
    -dC:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\DATA\
    master.mdf;-eC:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\
    LOG\ERRORLOG;-lC:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\
    DATA\mastlog.ldf
    ```
    
    Wenn der geplante Speicherort für das Verschieben der Master- und Protokolldateien E:\\SQLData lautet, werden die Parameterwerte folgendermaßen geändert:
    
    ```
    -dE:\SQLData\master.mdf;-eC:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\LOG\ERRORLOG;-lE:\SQLData\mastlog.ldf
    ```
5. Beenden Sie die Instanz von SQL Server, indem Sie mit der rechten Maustaste auf den Instanznamen klicken und Beenden wählen.
6. Verschieben Sie die Dateien master.mdf und mastlog.ldf an den neuen Speicherort.
7. Starten Sie die Instanz von SQL Server neu.
8. Überprüfen Sie die Dateiänderung für die master-Datenbank, indem Sie die folgende Abfrage ausführen. ```
    SELECT name, physical_name AS CurrentLocation, state_desc
    FROM sys.master_files
    WHERE database_id = DB_ID('master');
    GO
    ```

</div></div></div># <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

[http://msdn.microsoft.com/de-de/library/ms345408.aspx](http://msdn.microsoft.com/de-de/library/ms345408.aspx)