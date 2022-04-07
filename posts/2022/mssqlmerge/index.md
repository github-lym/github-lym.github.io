# MSSQL比對兩資料表並Merge


感謝Warren大大讓我認識了這個指令。  
<!--more-->

當時需要塞資料表，Warren就說這用`Merge`比較快。  
比對兩資料表，比對不到的就塞，有比對到的就更新值。  

  

```sql
MERGE INTO MP02_ASIGNCASE as target --要被insert/update/delete的表
USING TMP_MP02_ASIGNCASE as source --被參考的表
	ON (target.FORM_LOG_ID = source.FORM_LOG_ID)  --比對條件
WHEN NOT MATCHED
	THEN
		INSERT
		VALUES (
			source.FORM_LOG_ID
			,source.TDOC_NO
			--中間省略  
			,source.UPD_DATE
			)
WHEN MATCHED
	THEN
		UPDATE
		SET target.TDOC_NO = source.TDOC_NO
			,target.TCASE_ADMIN_NAME = source.TCASE_ADMIN_NAME
            --中間省略  
			,target.UPD_DATE = source.UPD_DATE;  --結尾一定是分號
```
  
  
`OUTPUT`是用來讀取 inserted 和 deleted 這二個特殊資料表用的。  
```sql
INSERT INTO dbo.TestLog 
Select 
  mrg.* 
From 
  (
    MERGE INTO TestTarget as target --要被insert/update/delete的表
    USING TestSource as source --被參考的表
    ON (
      target.RegionID = source.RegionID
    ) WHEN NOT MATCHED THEN INSERT 
    VALUES 
      (
        source.RegionID, source.RegionDescription
      ) WHEN MATCHED 
      AND target.RegionDescription <> source.RegionDescription THEN 
    UPDATE 
    SET 
      target.RegionDescription = source.RegionDescription OUTPUT $action as MergeAction, 
      deleted.RegionID as _RegionID, 
      deleted.RegionDescription as _RegionDescription, 
      getdate() as dt
  ) AS mrg 
WHERE 
  mrg.MergeAction = 'UPDATE'
```  
  
```sql
MERGE INTO TestTarget as target --要被insert/update/delete的表
USING TestSource as source --被參考的表
ON (
  target.RegionID = source.RegionID
) WHEN NOT MATCHED THEN INSERT 
VALUES 
  (
    source.RegionID, source.RegionDescription
  ) WHEN MATCHED 
  AND target.RegionDescription <> source.RegionDescription THEN 
UPDATE 
SET 
  target.RegionDescription = source.RegionDescription OUTPUT $action, 
  deleted.RegionID, 
  deleted.RegionDescription, 
  getdate() into dbo.TestLog;

```
