# 節約CPU效能的Sleep

從來沒用過這東西，不過看到了就該筆記。  
<!--more-->
```CSharp
//常用的執行序休眠, 但耗效能
Thread.Sleep(10000);
//節約效能的執行序休眠
SpinWait.SpinUntil(() => false, 10000);
```
