# Join一對多資料只取第一筆

這應該不是我第一次有這需求。   
<!--more-->

不過有把它寫下來應該是第一次。  

<br>

重點在於`row_number()`取得編號，才能取第一筆。  
>以帳號`(clr_ca_account)`為區隔，以時間`(CLR_LogoutDT)`為排序。  
```SQL
SELECT  A.*,
A.CA_Account,A.ca_cum_bankcode+'-'+A.ca_name ,
R.CR_RoleName,IIF(A.CA_AccountDisable='Y','停用','啟用'),
A.CA_ModTime,CLR_LoginDT
FROM [dbo].[CMS_Account] as A
left join (SELECT clr_ca_account,CLR_LoginDT,row_number() over (partition by clr_ca_account order by CLR_LogoutDT desc) as rownum
FROM [dbo].[CMS_LoginRec]) AS L  on A.ca_account = L.clr_ca_account
AND L.rownum='1'
Left join [CMS_Role] AS R on A.CA_CR_RoleCode = R.CR_RoleCode
```
