# StoredProcedure撰寫與使用

不確定是不是第一次寫，反正就記錄一下。
<!--more-->
```SQL
CREATE PROCEDURE [dbo].[GetPushCode] (

        @BCODE VARCHAR(3)

        ,@UID VARCHAR(10)

        ,@PushCode int OUTPUT

        )

AS

BEGIN

        SET NOCOUNT ON;

        -- 查詢USUDID

        DECLARE @USUDID uniqueidentifier

        -- 裝置ID

        DECLARE @DEV_ID varchar(50)

        SELECT @USUDID = USUDID

        FROM [AgriBank].[dbo].[CUST]

        WHERE ID_DATA = @UID

                AND BR_CODE = @BCODE + '00'

        -- PRINT @USUDID

        SELECT TOP 1 @DEV_ID = AALR_DeviceID

        FROM [FFICM].[dbo].[API_APPLoginRec]

        WHERE AALR_C_USUDID = @USUDID

        ORDER BY AALR_LoginDT DESC

        -- PRINT @DEV_ID

        SELECT @PushCode = ALDI_PUSHCODE

        FROM [FFICM].[dbo].[API_LoginDeviceInfo]

        WHERE [ALDI_UID] = @DEV_ID

                AND [ALDI_C_USUDID] = @USUDID

END
```

然後C#呼叫為
```CSharp
                        SqlCommand sqlCommand = new SqlCommand("[FFICM].[dbo].[GetPushCode]", SqlConn);

                        sqlCommand.CommandType = CommandType.StoredProcedure;

                        sqlCommand.Parameters.AddWithValue("@BCODE", 帳號前三碼);

                        sqlCommand.Parameters.AddWithValue("@UID", 統一編號);

                        sqlCommand.Parameters.AddWithValue("@PushCode", 0);

                        sqlCommand.Parameters["@PushCode"].Direction = ParameterDirection.Output;

                        sqlCommand.ExecuteScalar(); //取得PushCode

 

                        int pushCode;

                        int.TryParse(sqlCommand.Parameters["@PushCode"].Value.ToString(), out pushCode);
```

<br>

用SQL呼叫的語法如下(取得output)
```SQL
DECLARE @myretValue int

EXEC GetPushCode @BCODE = '928', @UID = 'H2228xxxxxx', @PushCode = @myretValue output

select @myretValue;
```
<br>
用SQL呼叫取得兩output的語法如下

```SQL
DECLARE @pushcode int,@usudid uniqueidentifier

 

EXEC GetPushCode @BCODE = '928', @UID = 'H2228xxxxxx', @PushCode = @pushcode output,@USUDID = @usudid output

 

select @pushcode,@usudid;
```


