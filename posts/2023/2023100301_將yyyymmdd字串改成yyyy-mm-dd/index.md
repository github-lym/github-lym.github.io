# 將yyyyMMdd字串改成yyyy-MM-dd

簡單粗暴的做法就是轉成DateTime格式再轉。   
<!--more-->
有用過就先抄下來，避免以後忘記。   

```CSharp 
str_output = Datetime.ParseExact(str_input,“yyyyMMdd”,System.Globalization.CultureInfo.Invarianculture).ToString(“yyyy-MM-dd”);
```
