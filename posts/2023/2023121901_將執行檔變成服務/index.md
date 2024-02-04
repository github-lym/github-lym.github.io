# 將執行檔變成服務

因每次重開機都要重新執行檔案，所以去找有無方法變成服務。  
<!--more-->

想說能不能懶惰一點，結果找到以下方法。  
```cmd
:創立
sc create  <service_name> binpath= "<binary_path>"
:停止
sc stop    <service_name>
:查詢
sc queryex <service_name>
:刪除
sc delete  <service_name>
```
不過不知是否我權限不足，出現了失敗的訊息。  
[![失敗](../img/2023121901_01.png '失敗')](../img/2023121901_01.png)  
有機會再試試。  


