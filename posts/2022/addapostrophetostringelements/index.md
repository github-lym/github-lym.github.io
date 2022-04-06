# 將字串元素加雙引號


`string v = "10,14,18,21";`

```CSharp
//method1
var r1 = string.Join(",", v.Split(",").Select(x => $"'{x}'"));
Console.WriteLine(r1); // '10', '14', '18', '21'

//method2
var r2 = $"'{v.Replace(",", "','")}'";
Console.WriteLine(r2);

//method3  using System.Text.RegularExpressions; 
string r3 = Regex.Replace(v, "[^,]+", "'$0'");
Console.WriteLine(r3);
```  

輸出為`'10','14','18','21'`
