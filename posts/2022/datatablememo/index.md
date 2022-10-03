# DataTable的小技巧

偶爾看到兩個小技巧，筆記一下。
<!--more-->
1. Expression  
算是公式計算？  
```CSharp
void Main()
{
    string connectionString = @"Server=(LocalDB)\MSSQLLocalDB;Initial Catalog=Northwind;Integrated Security=True; MultipleActiveResultSets=True";

    using (SqlConnection con = new SqlConnection(connectionString))
    {
        using (SqlCommand cmd = new SqlCommand(@"Select top 100 * from dbo.[Order Details]", con))
        {
            cmd.CommandType = CommandType.Text;
            using (SqlDataAdapter sda = new SqlDataAdapter(cmd))
            {
                using (DataTable dt = new DataTable())
                {
                    sda.Fill(dt);
                    DataColumn dc = new DataColumn("c2", Type.GetType("System.Int32"));
                    dc.Expression = "Quantity * 3";
                    dt.Columns.Add(dc);
                    dt.Dump();
                }
            }
        }
    }
}
```
可以把欄位做運算或判斷處理。  
  
2. 可以反悔的編輯功能與 DataRow 狀態  
` lname = dr["LastName", DataRowVersion.Original];`  
比較常用應該是`Original`、`Current`，  
配合`BeginEdit`尚未`EndEdit`前，則又可以使用`Proposed`。  
