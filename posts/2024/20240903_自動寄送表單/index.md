# 自動寄送表單

就為了查調程式功能。    
<!--more-->

就花點時間把[這個](https://github.com/github-lym/CorpEBMonthlyFunction)寫出來。  

<br>

比較特別的是，原來可以把`DataTable`直接匯出至`ClosedXML`成Excel，  
標題過濾還幫你弄得好好的。  
mail則能直接把記憶體內的內容附加寄出。 

<br>
 

```CSharp
wbook.Worksheets.Add(dt, "Sheet1"); //直接把DB匯入excel
var worksheet = wbook.Worksheet(1);
worksheet.Columns().AdjustToContents(); //根據cell大小調整寬度
wbook.SaveAs(ms);
SendMail(ms);

(中略)

string filename = $"新企網銀單位功能啟用清單 {today.ToString("yyyyMM")} 版" + ".xlsx";
ms.Position = 0;
var att = new System.Net.Mail.Attachment(ms, filename, "application/vnd.ms-excel");
message.Attachments.Add(att);

```  
<br>  

[![結果](../img/20240903_01.jpg '結果')](../img/20240903_01.jpg.png) 


