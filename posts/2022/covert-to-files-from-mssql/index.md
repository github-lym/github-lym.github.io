# 將MSSQL Table的Binary檔轉出


當時臨時接到這個要求。  
<!--more-->

先掛載到Server

> 因Express無法掛超過10GB，請用其他進階版本。

```sql
USE [master]
GO
CREATE DATABASE [tms01p141] ON
( FILENAME = N'D:\DB\tms01p141.mdf' ),
( FILENAME = N'D:\DB\tms01p141_log.ldf' )
FOR ATTACH ;
GO
```

> 權限不夠不能執行的話請自行開放。

```sql
CREATE TABLE SUB (FOLDER int) --建一個臨時Table放id  

INSERT INTO SUB  
SELECT TOP 10 --筆數條件  
B.id as FOLDER  
FROM  
Binder as B,  
Document as D,  
Document_Attachment as DA,  
Attachment as A  
Where  
B.currentDoc_id=D.id  
AND D.id=DA.doc_id  
AND DA.at_id=A.id  

DECLARE @SQLIMG VARCHAR(MAX),  
@RAW_DATA VARBINARY(MAX), --檔案二進位資料欄位(存放rawdata)  
@FILENAME VARCHAR(MAX), --檔名欄位(記錄上傳檔名)  
@PATH VARCHAR(MAX), --最底層路徑  
@FOLDER VARCHAR(MAX), --資料夾  
@FILEID VARCHAR(MAX), --不重要  
@ObjectToken INT,  
@cmdpath nvarchar(60) --命令列  

SET @PATH = 'D:\' --設定最底層路徑  

--建資料夾BEGIN  
DECLARE C_SUB CURSOR FAST_FORWARD FOR  
SELECT FOLDER FROM SUB group by FOLDER  

OPEN C_SUB  
FETCH NEXT FROM C_SUB INTO @FOLDER  
WHILE @@FETCH_STATUS = 0  

BEGIN  
SET @FOLDER = @PATH + @FOLDER; --設定路徑  
SET @cmdpath = 'MD ' + @FOLDER  
EXEC master.dbo.xp_cmdshell @cmdpath  

FETCH NEXT FROM C_SUB INTO @FOLDER  
END  


CLOSE C_SUB  
DEALLOCATE C_SUB  
--建資料夾END  

DECLARE FILES CURSOR FAST_FORWARD FOR  
SELECT TOP 10 --筆數條件  
B.id as FOLDER, A.id as FileId, A.fileName as FILENAME, A.rawData as RAW_DATA  
FROM  
Binder as B, Document as D, Document_Attachment as DA, Attachment as A  
Where  
B.currentDoc_id=D.id  
AND D.id=DA.doc_id  
AND DA.at_id=A.id ---選擇欲轉出的資料(選擇rawdata跟檔名欄位)  


OPEN FILES  

FETCH NEXT FROM FILES INTO @FOLDER,@FILEID,@FILENAME,@RAW_DATA  

WHILE @@FETCH_STATUS = 0  
BEGIN  

SET @FOLDER = @PATH + @FOLDER; --設定路徑  
--SET @cmdpath = 'MD ' + @FOLDER  
--EXEC master.dbo.xp_cmdshell @cmdpath  
SET @FILENAME = @FOLDER + '\' + @FILENAME  

PRINT @FILENAME  
--PRINT @SQLIMG  

EXEC sp_OACreate 'ADODB.Stream', @ObjectToken OUTPUT  
EXEC sp_OASetProperty @ObjectToken, 'Type', 1  
EXEC sp_OAMethod @ObjectToken, 'Open'  
EXEC sp_OAMethod @ObjectToken, 'Write', NULL, @RAW_DATA  
EXEC sp_OAMethod @ObjectToken, 'SaveToFile', NULL, @FILENAME, 2  
EXEC sp_OAMethod @ObjectToken, 'Close'  
EXEC sp_OADestroy @ObjectToken  

FETCH NEXT FROM FILES INTO @FOLDER,@FILEID,@FILENAME,@RAW_DATA  
END  

CLOSE FILES  
DEALLOCATE FILES  

DROP TABLE SUB  
```
