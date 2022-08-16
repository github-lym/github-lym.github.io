# 取得登錄檔的值


本來是要用預設瀏覽器開啟網址，但看到另種處理方法。

<!--more-->

這方法是去登錄檔找 HTTP 是何執行檔開啟的值。

```CSharp
static string browser = string.Empty;
static RegistryKey key = null;

key = Registry.ClassesRoot.OpenSubKey(@"HTTP\shell\open\command");
browser = key.GetValue(null).ToString().ToLower().Trim(new[] { '"' });

if (!browser.EndsWith("exe"))
{
	//Remove all after the ".exe"
	browser = browser.Substring(0, browser.LastIndexOf(".exe", StringComparison.InvariantCultureIgnoreCase) + 4);
}
```

