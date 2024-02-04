# List泛型的Distinct

本以為刪除重複用`Distinct`就簡單搞定了。  
<!--more-->
後來才知道因為List泛型內容物的關係都有獨立的參考，這樣是不行的。  

<br>

最終查到用`GroupBy`再`Select`選擇第一筆搞定。  
```CSharp
result = result.OrderBy(x => x.login_date).ThenBy(x => x.login_time).GroupBy(x => new { x.ca_id, x.acct, x.login_date, x.login_time, x.login_ip, x.memo }).Select(g => g.First()).ToList();
```  
<br>
<br>

### 20240202更新1  
後來查到`DistinctBy`可用，但.Net的版本`(.NET 6)`可能得注意。  
```CSharp
result = result.OrderBy(x => x.login_date).ThenBy(x => x.login_time).DistinctBy(x => new { x.ca_id, x.acct, x.login_date, x.login_time, x.login_ip, x.memo }).ToList();
```  
### 20240202更新2  
後來又查到若.Net版本不夠可以自己寫擴充，這些人真的太優秀了。  
```CSharp
public static class DistinctByClass
{
	public static IEnumerable<TSource> DistinctBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector)
	{
		HashSet<TKey> seenKeys = new HashSet<TKey>();
		foreach (TSource element in source)
		{
			if (seenKeys.Add(keySelector(element)))
			{
				yield return element;
			}
		}
	}
}
```  
