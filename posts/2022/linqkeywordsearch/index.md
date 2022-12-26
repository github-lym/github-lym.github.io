# 查詢字串裡面的多個關鍵字

看了[這篇](https://dotblogs.com.tw/kevinya/2022/12/12/115149)才知有這種使用方式。  
<!--more-->
```CSharp
using (MyEntities db = new MyEntities())
{
	List<string> keyWordList = new List<string>();
	//變數model.SearchKeyWord是在畫面上，使用者輸入的關鍵字查詢
	//可輸入單一關鍵字，也可跟google查詢一樣輸入多個關鍵字（以空格分隔多個關鍵字）
	//例如：sony 手機 旗艦機
	if (!string.IsNullOrEmpty(model.SearchKeyWord))
	{
		keyWordList = model.SearchKeyWordd.Trim().Split(' ').ToList();
	}
	var query = from m in db.tblMyTable
					//關鍵字ToLower()的部分，可自行決定是否加上去（會導致查詢效能變差）
				where keyWordList.Count() > 0 ?
				  keyWordList.All(w => m.ColumnNo1.Contains(w)) : true
				//沒有ToLower版本（查詢效能較好）

				//有ToLower版本（查詢效能差很多，但有時候是有他的必要性在）
				//All(w => m.ColumnNo1.ToLower().Contains(w.ToLower())) : true
				select new MyModel
				{
					ColunmA = m.ColumnNo1,
					ColunmB = m.ColumnNo2,
					ColunmC = m.ColumnNo3,
				};
}
```
讓我驚豔的是這段：  
`where keyWordList.Count() > 0 ? keyWordList.All(w => m.ColumnNo1.Contains(w)) : true`  
如果有條件，則讓所有條件符合；若沒條件，則直接為`true`。  
才發現原來條件式可以這麼搞，算長了見識。

