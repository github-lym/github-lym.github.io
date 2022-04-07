# bcp匯出


駐點時要每天從資料庫匯出資料，就找到了這些。  
<!--more-->

要先啟用`xp_cmdshell`。  
```sql
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1;  
GO  
-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1;  
GO  
-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO  
```


```sql
--匯出查詢結果
DECLARE @sql varchar(2000),
@today varchar(8),
@filename varchar(200)
SET @today = convert(varchar, getdate()-1, 112)
SET @sql = 'SELECT ''員警編號'',''員警所屬單位'',''身分證(加密資料)'',''查核日期時間'',''查
核日期'',''查核時間'' ,''受查核人員進場時間'' ,''場館名稱'',''稽核結果'',''紀錄來源'' union all
SELECT cast([員警編號] as nvarchar(50)),cast([員警所屬單位] as nvarchar(50)),cast([身分
證(加密資料)] as nvarchar(50)),convert(varchar, [查核日期時間] , 120),cast([查核日期] as
nvarchar(50)),convert(varchar, [查核時間], 108),convert(varchar, [受查核人員進場時間],
120),cast([場館名稱] as nvarchar(50)),cast([稽核結果] as nvarchar(50)),cast([紀錄來源] as
nvarchar(50)) FROM [IOC5G].[dbo].[實名制員警稽查紀錄_查詢用] '
set @filename = 'D:\Output\' + @today +'.csv'
set @sql = 'bcp "' + @sql + '" queryout "' + @filename +'" -c -r"\n" -t"," -S Server名稱 -U 帳號 -P 密碼 -c -t, -T'
--print(@sql)
exec master..xp_cmdshell @sql
```  
  
```sql
--匯出Table Schema
DECLARE @sql varchar(2000),
@today varchar(8),
@filename varchar(200)
SET @today = convert(varchar, getdate(), 112)
SET @filename = 'D:\_TEST\' + @today +'.xml'
SET @sql = 'bcp Northwind.dbo.TestTarget format nul -n -t -x -f ' + @filename +' -c -S Server名稱 -U 帳號 -P 密碼 '
print(@sql)
exec master..xp_cmdshell @sql
```
