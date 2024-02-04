# Chrome你的連線不是私人連線解決方法？

先說結論，我失敗了。  
<!--more-->

因為IIS的SSL憑證過期，想要執行程式有時會跳出`你的連線不是私人連線`。  
想說本想設定一下Chrome，找到了以下的設定方式。  
`chrome://flags/#allow-insecure-localhost`  
<br>

但`Allow invalid certificates for resources loaded from localhost.`這選項我一直找不到。  
  
查了半天才知道藏在`M118`裡(當時版本)。  
[![失敗](../img/2024010501_01.png '失敗')](../img/2024010501_01.png)  

<br>

但還是失敗，還是不時會出現`你的連線不是私人連線`。  

<br>

最後還是更換了IIS的SSL憑證搞定。

