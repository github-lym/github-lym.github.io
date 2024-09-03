# 取得執行檔路徑相關


取得執行檔執行資料夾 


以`D:\Documents\bin\Debug\test.exe` 執行為例：  

```CSharp
string assemblyPath = Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);  //d:\documents\bin\Debug
```

取得執行檔完全路徑(含執行檔)

```CSharp
System.Reflection.Assembly.GetEntryAssembly().Location;  // d:\documents\bin\Debug\test.exe  
```
<br/>

若已有完整參考路徑`(assemblyPath)`：  
取得目前資料夾名稱  

```CSharp
new DirectoryInfo(assemblyPath).Name;  // Debug

new DirectoryInfo(assemblyPath).FullName;  // d:\documents\bin\Debug
```  

取得上一層路徑

```CSharp
Directory.GetParent(assemblyPath.TrimEnd(Path.DirectorySeparatorChar)).Name;  
//結果=>bin

Directory.GetParent(assemblyPath.TrimEnd(Path.DirectorySeparatorChar)).FullName;  
//結果=>d:\documents\bin
```

<br/>

***
以下內容為黑暗執行緒大大提供  https://blog.darkthread.net/blog/dotnet-io-path-funcs/
  
```CSharp  
Path.GetDirectoryName(@"D:\Documents\bin\Debug\test.exe"); // D:\Documents\bin\Debug  
Path.GetDirectoryName(@"\bin\Debug\test.exe"); // \bin\Debug  
Path.GetFileName(@"\bin\Debug\test.exe"); // test.exe  
Path.GetFileNameWithoutExtension(@"D:\Documents\bin\Debug\test.exe"); // test  
Path.GetExtension(@"D:\Documents\bin\Debug\test.exe"); // .exe  
Path.GetFullPath(@"D:\Documents\bin\Debug\test.exe") //  D:\Documents\bin\Debug\test.exe
Path.GetFullPath(@".\bin\Debug\test.exe")  //  會依當時路徑算出
```
<br/>

檢查路徑是否為從根目錄起始(絕對路徑)  
```CSharp  
Path.IsPathRooted(@"C:\Temp"); // true
Path.IsPathRooted(@"\Temp"); // true
Path.IsPathRooted(@".\test.txt"); // false
```

<br/>

取得根目錄 
```CSharp  
Path.GetPathRoot(@"Temp\test.txt"); // 空字串
Path.GetPathRoot(@"\Temp\test.txt"); // \
Path.GetPathRoot(@"C:\Temp\test.txt"); // C:\
Path.GetPathRoot(@"\\my-server\share\folder1\test.txt"); // \\my-server\share
```

<br/>

更換副檔名 
```CSharp  
Path.ChangeExtension(@"C:\Temp\test.txt", ".jpg"); // C:\Temp\test.jpg
```

<br/>

取得暫存目錄、暫存檔名(隨機產生且不重複) 
```CSharp  
Path.GetTempPath(); // 例：C:\Users\Jeffrey\AppData\Local\Temp\
Path.Combine(Path.GetTempPath(), Guid.NewGuid().ToString()); // 可拋式暫存檔
Path.GetTempFileName(); // 不重複隨機檔名，如：C:\Users\Jeffrey\AppData\Local\Temp\tmpB913.tmp
```

<br/>

檢查路徑或檔名是否包含無效字元 
```CSharp  
@"C:\Temp\".IndexOfAny(Path.GetInvalidPathChars()); // -1
@"C:\?Temp""\".IndexOfAny(Path.GetInvalidPathChars()); // 8, ? 合法 " 不合法
@"test?.txt".IndexOfAny(Path.GetInvalidFileNameChars()); // 4
```

<br/>

路徑相關字元 
```CSharp  
Path.DirectorySeparatorChar // \
Path.AltDirectorySeparatorChar // /
Path.PathSeparator // ;
Path.VolumeSeparatorChar // :
```
