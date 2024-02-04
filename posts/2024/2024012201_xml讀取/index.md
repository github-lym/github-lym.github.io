# XML讀取

查網路的結果，`XmlReader`最快。    
<!--more-->

我也東抄西抄搞定。
```CSharp
using (XmlReader rdr = XmlReader.Create(new StringReader(source.Rows[i]["MSG_CONTENT"].ToString())))
{
	string bankcode = string.Empty;
	while (rdr.Read())
	{
		if (rdr.NodeType == XmlNodeType.Element)
		{
			string elementName = rdr.Name;
			if ("ATMP".Equals(targetId) && elementName == "CARD_ACTNO")
			{
				bankcode = rdr.ReadElementContentAsString();
				if (bankcode.StartsWith("00"))  //如果是00開頭 去掉取三碼
					bankcode = bankcode.Substring(2, 3);
				else  //正常取三碼
					bankcode = bankcode.Substring(0, 3);

				break;
			}
			else if ("SystemF".Equals(targetId) && elementName == "KINBR")
			{
				bankcode = rdr.ReadElementContentAsString(); //正常取KINBR
				if (string.IsNullOrWhiteSpace(bankcode.Trim()) || bankcode.StartsWith("09"))  //若KINBR為空白或09開頭  下一行取ACBRNO
					bankcode = rdr.ReadElementContentAsString();    //取ACBRNO

				bankcode = bankcode.Substring(0, 3);    //取前三碼

			}
			else if ("SystemF".Equals(targetId) && elementName == "BASIC-KINBR")    //KINBR另一種變形
			{
				bankcode = rdr.ReadElementContentAsString().Substring(0, 3);
				if (bankcode.StartsWith("09"))
					continue;
			}

			if (bankcode.StartsWith("09") && elementName == "ACBRNO")  //若BASIC-KINBR為09開頭  就取ACBRNO
				bankcode = rdr.ReadElementContentAsString().Substring(0, 3);    //取ACBRNO
		}

	}
}
```

<br>

但`XmlReader`最大的問題是，讀完`(ReadElementContentAsString)`一個`Element`後，  
會自動移到下一個Element。  
這個貼心(?)這設定讓我一度懷疑我寫錯程式。  
直到在網路上查到遇到相同情況的同志。  
而且是依序讀取，操作多有不便，除非你的檔案格式比較單純。  

<br>

所以網路上還是推薦`XmlDocument`，相形下自由許多。  
```CSharp
XmlDocument xmlDoc = new XmlDocument();
xmlDoc.LoadXml(source.Rows[i]["MSG_CONTENT"].ToString());

//兩種取法(不過我第一種沒成功)
string UserName = xmlDoc.SelectSingleNode("/RESPONSE/HEADER/UserName").InnerText;

//xmlDoc.GetElementsByTagName("KINBR")  //取得為XmlNodeList 所以實際使用如下
string bankcode = xmlDoc.GetElementsByTagName("CARD_ACTNO")[0].InnerText;
```


