# Hugo常用筆記


隔了四個月，差點連指令都不會打了。

<!--more-->

不用快取啟動 Server

```cmd
hugo server --disableFastRender
```
  
只執行 `hugo` 會匯出編譯檔在`public`資料夾。
產生出的`index.json`拿去[Algolia](https://www.algolia.com/ 'algolia')建立搜尋索引。

