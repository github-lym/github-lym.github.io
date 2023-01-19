# 讀取ini檔

以前用網路上看的，比較複雜。
<!--more-->

今天網路上看到用Windows內建的省事多了。  
上班有空就[做了出來](https://github.com/github-lym/IniManager)。  


不過在測試刪除功能怎麼測都失敗。  
後來才發現刪除參數要用`null`，範例卻ToString()。   
難怪會發生錯誤。
