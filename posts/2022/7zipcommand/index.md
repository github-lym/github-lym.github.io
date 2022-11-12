# 7-Zip排除特定目錄壓縮

要排除某些資料夾壓縮，本來只有找到壓成7z的指令。
<!--more-->
`7z a MyProject.7z MyProject –mx7 –r –xr@proj-ignore.txt`  
\
後來找到壓成ZIP格式的指令。  
`7z a -tzip D:/Code/CMS_WebSite.zip D:/Code/CMS_WebSite -xr@D:/Code/proj-ignore.txt`  
\
那`proj-ignore.txt`內容為  
```
.git
bin
obj
packages
.vs
.vscode
Logs
```
