# 取得執行檔資料夾與完整路徑


取得執行檔執行資料夾 


以`D:\Documents\bin\Debug\test.exe` 執行為例：  

```CSharp
//結果=>d:\documents\bin\Debug
string assemblyPath = Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
```

取得執行檔完全路徑(含執行檔)

```CSharp
//結果=>d:\documents\bin\Debug\test.exe
System.Reflection.Assembly.GetEntryAssembly().Location;
```
<br/>

若已有完整參考路徑`(assemblyPath)`：  
取得目前資料夾名稱  

```CSharp
//結果=>Debug
new DirectoryInfo(assemblyPath).Name;

//結果=>d:\documents\bin\Debug
new DirectoryInfo(assemblyPath).FullName;
```  

取得上一層路徑

```CSharp
//結果=>bin
Directory.GetParent(assemblyPath.TrimEnd(Path.DirectorySeparatorChar)).Name;

//結果=>d:\documents\bin
Directory.GetParent(assemblyPath.TrimEnd(Path.DirectorySeparatorChar)).FullName;
```
