# SqlBulkCopy—快速寫入MSSQL資料庫


之前在要在資料庫寫入數十萬乃至數百萬的資料，以往都是一筆筆慢慢寫入。    
<!--more-->

後來Warren提點才知道有個**SqlBulkCopy**可以用，省時太多了。  
\
當時在駐點開發的小專案，本來耗時十幾分鐘的資料，  
不到一分鐘就完工了！  
\
這個跟MSSQL的[Merge](/posts/2022/mssqlmerge/)搭配，讓我在駐點那時搞定了不少小排程程式。 
```CSharp
using (SqlBulkCopy bulkcopy = new SqlBulkCopy(_dao._conn))
{
	//目標資料庫名稱
	bulkcopy.DestinationTableName = "MP01_ASIGNCASE";
	//逾時秒數
	bulkcopy.BulkCopyTimeout = 60;
	//設定每個批次要複製的筆數
	bulkcopy.BatchSize = 20000;
	//設定當SqlBulkCopy複製多少筆資料後, 觸發通知事件
	bulkcopy.NotifyAfter = 200000;
	bulkcopy.SqlRowsCopied += new SqlRowsCopiedEventHandler(sqlBulkCopy_SqlRowsCopied);

	//column對應
	bulkcopy.ColumnMappings.Add("稽查表單編號", "FORM_LOG_ID");
	bulkcopy.ColumnMappings.Add("公文文號", "TDOC_NO");
	bulkcopy.ColumnMappings.Add("承辦人", "TCASE_ADMIN_NAME");

	bulkcopy.WriteToServer(dt);
	bulkcopy.Close();
}


//SqlTransaction版本
using (SqlConnection conn = new SqlConnection(connectionString))
{
	conn.Open();
	using (SqlTransaction transaction = conn.BeginTransaction())
	{
		using (SqlBulkCopy bulkcopy = new SqlBulkCopy(conn, SqlBulkCopyOptions.Default, transaction))
		{

			//目標資料庫名稱
			bulkcopy.DestinationTableName = "EAI_TranRecStatistics";
			//逾時秒數
			bulkcopy.BulkCopyTimeout = 60;
			//設定每個批次要複製的筆數
			bulkcopy.BatchSize = 100000;
			//設定當SqlBulkCopy複製多少筆資料後, 觸發通知事件
			//bulkcopy.NotifyAfter = 200000;
			//bulkcopy.SqlRowsCopied += new SqlRowsCopiedEventHandler(sqlBulkCopy_SqlRowsCopied);
			try
			{
				bulkcopy.WriteToServer(target);
				transaction.Commit();
				logger.Info($"匯入{sDate}至{eDate}資料至資料庫完成");
			}
			catch (Exception ex)
			{
				logger.Info($"匯入{sDate}至{eDate}資料至資料庫失敗");
				logger.Error("ImportToDb(bulkcopy):" + ex.ToString());
				transaction.Rollback();
			}
			finally
			{
				bulkcopy.Close();
			}
		}

		conn.Close();
	}

}


static void OnSqlRowsCopied(object sender, SqlRowsCopiedEventArgs e)
{
	Console.WriteLine("Copied {0} so far...", e.RowsCopied);
}
```


