# Git從其他分支Copy檔案到工作目錄


今天Blog用了兩個主題，  
所以需要不停切換。
<!--more-->

有需要把其他分支的檔案copy到主分支，  
查了一下，發現在Stack Overflow網站的答案非常棒。  
```Bash
git checkout 其他分支 .
```
不過因為是copy的關係，若有相同檔案會直接覆蓋而不會產生衝突(conflict)。  
這動作目前在tortoisegit我還沒發現該怎麼做，  
git指令操作果然還是王道。  
  
*另外補充只要某檔案：*  
```Bash
git checkout 其他分支 -- 檔案名
```
