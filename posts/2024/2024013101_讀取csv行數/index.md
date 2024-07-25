# 讀取csv行數

就為了統計查調程式查詢的筆數。    
<!--more-->

就花點時間把[這個](https://github.com/github-lym/CIBNPA_Statistics)寫出來。  

<br>

新嘗試是用了`Lambda`的`Any`和`RemoveAll`，  
還有`KeyValuePair`無法更新Value數值，只能移除再重寫。  
以及用`正規表示法 (?<=.*_)\d{3}(?=_)`，  
取得`D11210000900_928_B1_20231002_OK5`中間三位機關代碼數字。

