# MSSQL備份相關


跟備份相關的。  
<!--more-->

>備份所有DB
>>```sql
>>DECLARE @name VARCHAR(50) -- database name
>>DECLARE @path VARCHAR(256) -- path for backup files
>>DECLARE @fileName VARCHAR(256) -- filename for backup
>>DECLARE @fileDate VARCHAR(20) -- used for file name
>>-- specify database backup directory
>>SET @path = 'C:\Backup\'
>>-- specify filename format
>>SELECT @fileDate = CONVERT(VARCHAR(20),GETDATE(),112)
>>DECLARE db_cursor CURSOR READ_ONLY FOR
>>SELECT name
>>FROM master.sys.databases
>>WHERE name NOT IN ('master','model','msdb','tempdb') -- exclude these >>databases
>>AND state = 0 -- database is online
>>AND is_in_standby = 0 -- database is not read only for log shipping
>>OPEN db_cursor
>>FETCH NEXT FROM db_cursor INTO @name
>>WHILE @@FETCH_STATUS = 0
>>BEGIN
>>SET @fileName = @path + @name + '_' + @fileDate + '.BAK'
>>BACKUP DATABASE @name TO DISK = @fileName
>>FETCH NEXT FROM db_cursor INTO @name
>>END
>>CLOSE db_cursor
>>DEALLOCATE db_cursor
>>```  
  
  
  
  
>壓縮所有Log檔
>>```sql
>>SET nocount ON
>>
>>SELECT 'USE [' + d.NAME + N']' + Char(13) + Char(10)
>>       + 'DBCC SHRINKFILE (N''' + mf.NAME
>>       + N''' , 0, TRUNCATEONLY)' + Char(13) + Char(10)
>>       + Char(13) + Char(10)
>>FROM   sys.master_files mf
>>       JOIN sys.databases d
>>         ON mf.database_id = d.database_id
>>WHERE  d.database_id > 4
>>       AND mf.type_desc = 'LOG' 
>>```  
  
  
  
  
>移資料庫位置
>>```sql
>>USE master; --do this all from the master
>>
>>ALTER DATABASE covid19
>>
>>SET offline WITH
>>
>>ROLLBACK immediate;
>>
>>ALTER DATABASE covid19 modify FILE (NAME='COVID19', filename= >>'D:\_MSSQL_DB\COVID19.mdf');
>>
>>ALTER DATABASE covid19 modify FILE (NAME='COVID19_LOG', filename= >>'D:\_MSSQL_DB_LOG\COVID19_log.ldf');
>>
>>--然後移動實體檔案
>>ALTER DATABASE COVID19 SET ONLINE;
>>```

