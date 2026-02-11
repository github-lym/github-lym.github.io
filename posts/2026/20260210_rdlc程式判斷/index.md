# RDLC程式判斷

RDLC遇到一個欄位，要不就空白，要不就以分號分割換行。  
<!--more-->

結果一開始不管怎麼試欄位運算式，空白的欄位都會顯示'**#錯誤**'。   

```
=IIF(IsNothing(Fields!TFER_TYPE.Value), "",  Split(Fields!TFER_TYPE.Value, ";")(0) & System.Environment.NewLine & Split(Fields!TFER_TYPE.Value, ";")(1))

=IIF(Len(Fields!TFER_TYPE.Value) = 0, Fields!TFER_TYPE.Value,  Split(Fields!TFER_TYPE.Value, ";")(0) + System.Environment.NewLine + Split(Fields!TFER_TYPE.Value, ";")(1))
```
   
<br>

後來上網找到說可以在報表屬性自訂程式碼。***(rdlc report properties custom code)***   
但要用V寫，還好有得參考照抄。   

```VB
Public Function SplitCode(Data As String) As String

        If String.IsNullOrEmpty(Data) Then
            Return ""
        End If

        Return Split(Data , ";")(0) & vbNewLine & Split(Data , ";")(1) 

End Function

``` 
<br>  

然後在欄位的運算式設定：
```
=Code.SplitCode(Fields!TFER_TYPE.Value)
```


<br>

終於解決我的問題。

