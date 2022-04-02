# 使用robocopy站台備份


站台要更新之前一定得備份。  

<!--more-->
之前土炮的作法，當然是在備份區建立一個日期資料夾，然後複製貼上。  
\
這麼做也不是不行，但logs資料其實沒必要備份，  
再加上使用windows複製不夠有效率，我又把腦筋動到了robocopy上。  


```bash
:@Echo Off
Set "sd=C:\inetpub\CMS"
Set "dd=C:\inetpub\CMS_BACKUP"
Set "ex1=C:\inetpub\CMS\WebApi\Logs"

:這段是用來產生當日日期
for /f "tokens=2 delims==" %%a in ('wmic OS Get localdatetime /value') do set "dt=%%a"
set "YY=%dt:~2,2%" & set "YYYY=%dt:~0,4%" & set "MM=%dt:~4,2%" & set "_DD=%dt:~6,2%"
set "HH=%dt:~8,2%" & set "Min=%dt:~10,2%" & set "Sec=%dt:~12,2%"
set "datestamp=%YYYY%%MM%%_DD%" & set "timestamp=%HH%%Min%%Sec%" & set "fullstamp=%YYYY%-%MM%-%_DD%_%HH%%Min%-%Sec%"
:echo datestamp: "%datestamp%"
:echo timestamp: "%timestamp%"
:echo fullstamp: "%fullstamp%"

RoboCopy "%sd%" "%dd%\CMS_BACKUP_%datestamp%\CMS" /MIR /E /R:5 /W:5 /TBD /NP /NFL /V /MT:32 /XD "%ex1%"

pause
```
一開始就設定好來源與目標，還有排除的資料夾。
```
/mir 鏡像目錄樹狀結構 (相當於 /e plus /purge) 。 使用這個選項搭配 /e 選項和目的地目錄，會覆寫目的地目錄安全性設定。  
/e	複製子目錄。 此選項會自動包含空的目錄。  
/s	複製子目錄。 此選項會自動排除空白目錄。  
/r:<n>	指定失敗複製的重試次數。N的預設值為 1000000 (1000000 重試)  
/w:<n>	指定重試之間的等待時間 (以秒為單位)。 N的預設值是 30 (等候時間30秒)  
/tbd 指定系統將等待定義共用名稱 (重試錯誤 67) 。  
  :看不懂，英文是寫這樣 `Wait for share names To Be Defined (retry error 67)
/np 指定將不會顯示複製作業的進度 (到目前為止複製的檔案或目錄數目)。` 速度比較快  
/nfl 指定不會記錄檔案名稱。
/v	產生詳細資訊輸出，並顯示所有略過的檔案。
/MT[:n]	使用 n 個執行緒建立多執行緒複製。
/xd <directory>[ ...]	排除符合指定名稱和路徑的目錄。
```
其實在寫這篇我才比較了解，還有用錯的。  
\
還是去[微軟官方](https://docs.microsoft.com/zh-tw/windows-server/administration/windows-commands/robocopy)看比較詳盡。
