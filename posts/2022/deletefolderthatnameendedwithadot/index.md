# 刪除名稱尾端為點(.)的資料夾


今天解壓縮，不小心輸出名稱多了一個點(.)。  

<!--more-->

就是`D:\Downloads\ScreenToGif.`。  

不管怎樣都刪不掉，於是上網找資料。  

發現這種命名在Windows下並不合法，所以無法做任何操作。  

好在我不是第一個，網上也有了解法。  

`rd /s "\\?\D:\Downloads\ScreenToGif.`

處理檔案的：  
`del "\?C:TempStuffSales Agreement."` <=  沒成功過 
`del \\?\E:\Downloads\mp4*.` <= 沒試過  
  
檔案還可以用壓縮軟體的檔案總管刪，親測有效！  

同篇文章說Win7的另一種操作：  
`del c:tempsomefil*`

