# 將DataTable的欄位名稱取出


已忘記何時用到的，不過還是記一下。

<!--more-->

```CSharp
string[] columnNames = inputDt.Columns.Cast<DataColumn>().Select(x => x.ColumnName).ToArray();
```

