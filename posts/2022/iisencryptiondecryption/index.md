# IIS加解密

雖知道有這功能，但之前完全沒有自己用過。
<!--more-->
先切換到aspnet_regiis.exe的執行位置。  
(若已經設定過環境變數可以直接找到指令就略過這個步驟)  

`cd C:\Windows\Microsoft.NET\Framework\v4.0.30319`  
  
`aspnet_regiis.exe -pef "connectionStrings" "C:\FFICMAPI"` 加密  
`aspnet_regiis.exe -pdf "connectionStrings" "C:\FFICMAPI"` 解密  
  
以前都一直以為路徑要直接包含web.config檔案，  
實際操作才知道這樣反而會失敗。
路徑到該IIS目錄即可。
