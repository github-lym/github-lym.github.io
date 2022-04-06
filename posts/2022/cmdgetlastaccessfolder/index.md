# 命令提示字元取得最後存取資料夾


*號表萬用字元，那邊可以寫路徑。  
```bash
for /f "delims=" %%a in ('dir /b /ad-h /od "*"') do set "latestDir=%%~a"
echo(%latestDir%
```   
