# 將IEnumerable轉成DataTable

使用linq，但回傳要是DataTable，所以找到並抄下來。  
<!--more-->
```CSharp
/// <summary>
/// 將IEnumerable轉成DataTable
/// </summary>
/// <typeparam name="T"></typeparam>
/// <param name="query"></param>
/// <returns></returns>
public static DataTable LinqQueryToDataTable<T>(IEnumerable<T> query)
{
	DataTable tbl = new DataTable();
	PropertyInfo[] props = null;
	foreach (T item in query)
	{
		if (props == null) //尚未初始化
		{
			Type t = item.GetType();
			props = t.GetProperties();
			foreach (PropertyInfo pi in props)
			{
				Type colType = pi.PropertyType;
				//針對Nullable<>特別處理
				if (colType.IsGenericType && colType.GetGenericTypeDefinition() == typeof(Nullable<>))
				{
					colType = colType.GetGenericArguments()[0];
				}
				//建立欄位
				tbl.Columns.Add(pi.Name, colType);
			}
		}
		DataRow row = tbl.NewRow();
		foreach (PropertyInfo pi in props)
		{
			row[pi.Name] = pi.GetValue(item, null) ?? DBNull.Value;
		}
		tbl.Rows.Add(row);
	}
	return tbl;
}
```
\
\
順便慶祝初次在實戰中使用`linq`將DataTable Left Join起來。  
```CSharp
var restultDt = from d in dt.AsEnumerable()
				join t in typeDt.AsEnumerable()
					 on d.Field<string>("FileName") equals t.Field<string>("FileName")
					 into groupjoin
				from a in groupjoin.DefaultIfEmpty()
				select new
				{
					Type = a.Field<string>("FileNameType"),
					SMSSendDate = d.Field<string>("SMSSendDate"),
					SwiftCod = d.Field<string>("SwiftCod"),
					BankName = d.Field<string>("BankName"),
					Phone = d.Field<string>("Phone"),
					Result = d.Field<string>("Result"),
					SMSSendTime = d.Field<DateTime>("SMSSendTime"),
				};
```
