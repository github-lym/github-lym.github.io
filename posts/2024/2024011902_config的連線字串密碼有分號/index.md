# Config的連線字串密碼有分號

config裡連線字串有分號很麻煩。    
<!--more-->

後來查到前後加單引號`'`搞定。
```CSharp
<add name="conn" providerName="System.Data.SqlClient" connectionString="Data Source=xxx.xxx.xxx.xxx;Initial Catalog=SomeDB;Persist Security Info=True;User ID=sa;Password='8ik,(OL>0p;/';" />  
```
