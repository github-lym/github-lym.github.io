# 取得執行檔資料夾與完整路徑


取得執行檔執行資料夾

```CSharp
string assemblyPath = Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
```

取得執行檔完全路徑

```CSharp
System.Reflection.Assembly.GetEntryAssembly().Location;
```

