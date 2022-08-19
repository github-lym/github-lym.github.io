# 問號的用法


因年紀大又沒常用老是忘記，記錄一下。

<!--more-->

```CSharp
string foo1 = a?.AddressLine ?? "N/A";  //a若為null，則foo1為"N/A"

int days = player.DaysSinceLastLogin.HasValue ? player.DaysSinceLastLogin.Value : -1;
//若player.DaysSinceLastLogin不為null
//則days為player.DaysSinceLastLogin.Value  否則為-1

int days = player.DaysSinceLastLogin ?? -1;
//跟上一例同義

string answer = five == 5 ? "true" : "false";
//若five為5 則answser為"true" 不為5則是"false"
```

