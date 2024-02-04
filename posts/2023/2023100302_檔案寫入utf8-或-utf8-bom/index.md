# 檔案寫入UTF8 或 UTF8 BOM

說實話，還是不清楚BOM的作用是啥。   
<!--more-->
雖然到處都查得到。  
>位元組順序記號（英語：byte-order mark，BOM）是位於碼點U+FEFF的統一碼字元的名稱。
>當以UTF-16或UTF-32來將UCS/統一碼字元所組成的字串編碼時，這個字元被用來標示其位元組序。
>它常被用來當做標示檔案是以UTF-8、UTF-16或UTF-32編碼的記號。
>資料擷取自維基百科

<br>

反正有用到，筆記一下。
```CSharp 
//產生UTF8 BOM
//Encoding utf8 = new UTF8Encoding(true);
//產生UTF8
//Encoding utf8 = new UTF8Encoding(false);

//實作
using (FileStream fileStream = new FileStream(utf8file, FileMode.Append, FileAccess.Write))
        {
            // Create a UTF-8 encoding.
            using (StreamWriter sw = new StreamWriter(fileStream, new UTF8Encoding(false)))
            {
                sw.WriteLine("Hello world! (UTF8)");
            }
        }
```
