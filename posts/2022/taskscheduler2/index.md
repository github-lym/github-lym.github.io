# 解決排程執行無效的問題


一般公司的主機常會有些排程。

<!--more-->

排程在某日後突然產生了`可以啟動但是實際上沒有執行`的怪事。

但實際登入執行又可以跑。

設定為`只有使用者登入`是沒問題啦…，但總覺得不夠好。

所以就把網路上可以嘗試的方法全混在一起做撒尿牛丸，果然就可以了。

1. 首先把排程的設定改為比較舊的版本。
   [![TaskScheduler](task01.webp '改用舊版本')](task01.webp)

2. 從 `[系統管理工具]` 開啟 `[本機安全性原則]`
   [![TaskScheduler](task02.webp '本機安全性原則')](task02.webp)

3. `[本機原則]` / `[使用者權限指派]` 並找到 `[以批次工作登入]` (Logon as a batch job) 選項，開啟設定視窗
   [![TaskScheduler](task03.webp '以批次工作登入')](task03.webp)

4. `[以批次工作登入 - 內容]` 視窗裡，點選 `[新增使用者或群組]` 按鈕，並新增使用者帳號或特定群組到這裡來。
   [![TaskScheduler](task04.webp '新增使用者或群組')](task04.webp)

好吧，這樣亂搞還居然有用。
