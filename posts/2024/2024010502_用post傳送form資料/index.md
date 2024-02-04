# 用Post傳送Form資料

本來`PostAsync`用得好好的，突然不行了。  
<!--more-->
如下方，就這麼突然不行了。  
```CSharp   
var content = new StringContent(strUrl, System.Text.Encoding.UTF8, "application/json");

string resultContent;
using (HttpClient httpClient = new HttpClient())
{
	System.Net.ServicePointManager.ServerCertificateValidationCallback = delegate { return true; };//是否忽略憑證
	ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
	var result = httpClient.PostAsync(strUrl, content).Result;
	resultContent = result.Content.ReadAsStringAsync().Result;
}
```   

題外話，我還找對方客服找到火大，因為一直找不到人。  
  
但後來找到有位客服跟我測試，用網頁Form的submit卻又沒問題。  
雖不知道差異在哪，反正就改成Form的樣式吧。  

<br>

改成這樣搞定。  

```CSharp   
string resultContent;
KeyValuePair<string, string>[] keyValues = new[]
{
	new KeyValuePair<string, string>("version", "1.0"),
	new KeyValuePair<string, string>("action", "bcv"),
	new KeyValuePair<string, string>("barCode", strMBarcode),
	new KeyValuePair<string, string>("TxID", Guid.NewGuid().ToString()),
	new KeyValuePair<string, string>("appId", MAPPID)
};

using (HttpClient httpClient = new HttpClient())
{
	System.Net.ServicePointManager.ServerCertificateValidationCallback = delegate { return true; };//是否忽略憑證																								
	ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
	var result = httpClient.PostAsync("https://xxx.xxx.nat.gov.tw/BIZAPIVAN/biz", formContent).Result;
	resultContent = result.Content.ReadAsStringAsync().Result;
	Console.WriteLine(resultContent);
	Console.ReadKey();
}
```
