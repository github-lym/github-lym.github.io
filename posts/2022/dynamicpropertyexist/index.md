# 判斷Dynamic某欄位是否存在

因傳遞給API的值要新增一欄位，為了新舊版的APP都要能用，  
<!--more-->
舊有做法是新增一API，  
讓新程式跑新API，舊程式跑舊API不受影響；  
但我看了看，新增API可能近十個！  
工程浩大，決定用修改現有的就好。  
  

用底下這來判斷dynamic是否有新欄位，  
然後在API新增判斷。
```CSharp
public static bool DynamicPropertyExist(dynamic settings, string name)
{
	try
	{
		var x = settings[name];
		if (x != null)
			return true;
		else
			return false;
	}
	catch
	{
		return false;
	}
}
```
<br/>

額外找到的`ExpandoObject`做法。  

```CSharp
public static bool DynamicPropertyExist(dynamic settings, string name)
{
	if (settings is ExpandoObject)
		return ((IDictionary<string, object>)settings).ContainsKey(name);

	return settings.GetType().GetProperty(name) != null;
}
```  



