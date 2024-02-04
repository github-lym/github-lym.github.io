# NLog用程式設定

以前還自己傻傻的自己寫Log處理。  
<!--more-->
直到發現`NLog`這東西，才知道人家高高手已經在遙遠星空看你在地上爬很久。  

<br>

NLog設定多樣化，會用程式是因為寫config的套件說不支援？  
[![不支援](../img/2024011901_01.png '不支援')](../img/2024011901_01.png)  

```CSharp
/// <summary>
/// Nlog設定
/// </summary>
private static void CreateLogger()
{
	var config = new LoggingConfiguration();
	var fileTarget = new FileTarget
	{
		FileName = "${basedir}/logs/${shortdate}.log",
		Layout = "${date:format=yyyy-MM-dd HH\\:mm\\:ss} [${uppercase:${level}}] ${message}",
		ArchiveEvery = FileArchivePeriod.Day,
		MaxArchiveFiles = 90

		//這邊是會把超過2048(2K)的檔案壓縮 最多就7個序號輪流
		//FileName = "${basedir}/logs/n.log",
		//Layout = "${date:format=yyyy-MM-dd HH\\:mm\\:ss} [${uppercase:${level}}] ${message}",
		//ArchiveAboveSize = 2048, //字節            
		//ArchiveNumbering = ArchiveNumberingMode.Rolling,
		//EnableArchiveFileCompression = true,
		//MaxArchiveFiles = 7
	};
	config.AddRule(LogLevel.Trace, LogLevel.Fatal, fileTarget);

	LogManager.Configuration = config;
}
```

要用時再呼叫。
```CSharp   
Logger logger = LogManager.GetCurrentClassLogger();
CreateLogger();
```

<br>

設定config應該還是方便修改，改天來研究。
