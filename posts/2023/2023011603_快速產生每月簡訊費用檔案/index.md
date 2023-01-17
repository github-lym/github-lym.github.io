# 快速產生每月簡訊費用檔案–LazyIVRMonthlySummary


公司每月都要統計報表。  

<!--more-->

因新舊資料庫問題，每次都得切換兩資料庫。  
執行stored procedure後，然後下不同指令，剪貼兩結果形成raw data。   

一時犯懶就[做了出來](https://github.com/github-lym/LazyIVRMonthlySummary)。  

有了這個就方便很多，起碼產生raw data不用切來切去，貼到含有公式的Excel就好。      
  
還練習了呼叫stored procedure。
  
有空再來把raw data轉換成Excel報表。  
  
---
20230117更新：  
因為今天有空檔，又加強了一下。  
再把raw data用ClosedXML繼續處理，  
讀取樣板產生Excel、CSV、Word檔。
