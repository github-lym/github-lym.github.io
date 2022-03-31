# 以FTP抓取資料庫備份


MSSQL資料庫每天都會備份，每週一得還原最新備份在另一台當歷史資料查詢。  
<!--more-->

通常資料庫備份容量不會太小，  
當時每週一早上都先進機房後，再花個一二十分鐘下載到指定主機再還原。  
一整個流程下來都要花個三四十分鐘。  
\
覺得這時間頗浪費，於是~~懶人病發作~~寫了[這個](https://github.com/github-lym/FTPdb)當排程。 
\
讓我可以在進機房之前就把備份檔下載好，起碼省去一半的時間。  
\
微軟[這邊](https://docs.microsoft.com/zh-tw/dotnet/framework/network-programming/how-to-upload-files-with-ftp)有上傳詳細教學：  
```csharp
using System;
using System.IO;
using System.Net;

namespace Examples.System.Net
{
    public class WebRequestGetExample
    {
        public static async Task Main()
        {
            // Get the object used to communicate with the server.
            FtpWebRequest request = (FtpWebRequest)WebRequest.Create("ftp://www.contoso.com/test.htm");
            request.Method = WebRequestMethods.Ftp.UploadFile;

            // This example assumes the FTP site uses anonymous logon.
            request.Credentials = new NetworkCredential("anonymous", "janeDoe@contoso.com");

            // Copy the contents of the file to the request stream.
            await using FileStream fileStream = File.Open("testfile.txt", FileMode.Open, FileAccess.Read);
            await using Stream requestStream = request.GetRequestStream();
            await fileStream.CopyToAsync(requestStream);

            using FtpWebResponse response = (FtpWebResponse)request.GetResponse();
            Console.WriteLine($"Upload File Complete, status {response.StatusDescription}");
        }
    }
}
```
我是下載，所以  
`request.Method = WebRequestMethods.Ftp.ListDirectory;`  
使用binary傳輸  
`request.UseBinary = true;`

