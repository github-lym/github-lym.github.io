# 用forfiles刪除過期檔案


之前有聽過forfiles的大名，但沒機會用。  

<!--more-->
這次襄理要我寫個批次檔刪除IIS上的logs，只保留一個月內的。  
\
立刻著手~~抄~~研究一下。  
  
我當時照抄了一份也試了覺得可行，  
事後發現在有子資料夾的情況下，只有刪除檔案，不會刪除子資料夾。  
也發現子資料夾的時間都被改變了，可能是刪除檔案的關係。  

```bash
forfiles -p "D:\logfiles" -m *.* -d -30 -c "cmd  /c del /q @path"
forfiles -p "D:\logfiles" -d -30 -c "cmd /c IF @isdir == TRUE rd /S /Q @path"
```  
到最後還是在網上找到答案。  
\
附上[微軟官方說明](https://docs.microsoft.com/zh-tw/windows-server/administration/windows-commands/forfiles)。

