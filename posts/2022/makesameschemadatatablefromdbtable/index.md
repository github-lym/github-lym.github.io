# 從資料表建相同的樣SchemaType的DataTable


之前還挺常用的，不過後來用 Entity 就沒在用了。

<!--more-->

```CSharp
	using (SqlConnection conn = new SqlConnection(connectionString))
	{
		using (SqlCommand cmd = conn.CreateCommand())
		{
			cmd.CommandText = "SELECT TOP 1 * FROM  TableName";
			conn.Open();
			DbDataAdapter da = new SqlDataAdapter(cmd);
			da.FillSchema(tmpDt, SchemaType.Source);
			conn.Close();
		}
	}
```

