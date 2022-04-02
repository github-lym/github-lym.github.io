# 批次執行vbs檔並檢查結果


剛進公司是駐點在公家機關做客服工程師，每天接電話回答系統問題。  

除此之外，每天下班為了流程都必須更新該機關組織圖。  
<!--more-->

前輩寫了一個很神的程式，他寫的vbs可以去抓資料庫的資料，  
並產生新的SQL指令(文字檔)可以更新組織圖。  
所以我們每天會檢查有異動的組織單位，產生該組織的SQL更新指令。   　

這產生的問題比較麻煩的點有兩個：  

    1. 有時資料庫連不上或是一些我不知道的理由，產生的SQL指令並不完全。  
    
    2. 要開啟多個文字檔執行SQL指令，頗浪費時間。做完一整套常花快一個鐘頭。  

### 於是，我的懶人病發作了，就寫了進公司的第一支程式。

一開始很單純，既然程式在有異動單位的資料夾產生*.sql檔，  
那我把子資料夾的檔案合併成一個再來執行就好。  
於是生成了[這個](https://github.com/github-lym/CombineAllSubFolderSQL)。  

雖省了一些時間，但並沒解決上述第一個問題。  

![](screenshot.png)  
上圖後來東問西抄問人寫出來的[第三版](https://github.com/github-lym/NTPC-Organizational-Chart)。  
\
\
當時從Leo那得知執行vbs的寫法：  
```csharp
private void _vbs(DirectoryInfo _folder)   //執行子目錄底下vbs
        {
            vbsfile = _folder.FullName + "\\" + set.VBSname;
            Process vbs = new Process();
            vbs.StartInfo.FileName = @"cscript";
            vbs.StartInfo.Arguments = " " + vbsfile;
            vbs.StartInfo.WorkingDirectory = _folder.FullName;
            vbs.Start();
            vbs.WaitForExit();
            vbs.Close();
            //System.Threading.Thread.Sleep(TimeSpan.FromTicks(set.waitTime));
        }
```
\
\
比以前的作業方式簡單又可檢查未產生的檔案並重新產出，  
算是個半全自動程式。
以前產生百個檔案以上無法一一檢查的問題迎刃而解，  
使用這版本以來，不再有未產出檔案的事發生。  

#### *但僅適用於本人上班場所，泛用性不高。*

**不過省了很多時間，本來一小時的操作變為大約20分鐘左右就搞定下班了。**

