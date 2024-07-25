# CompareList—產生副本檔案並修改時間


版更現在比對檔案越來越麻煩。  

<!--more-->

做出檔案差異檔，主測跟複測都得要一份。 

犯懶的我寫了[這個](https://github.com/github-lym/CompareList)。   
先做好一份比對檔案再執行即可。  

步驟就是先做出清單，  
把第一個原始比對檔案修改時間當基礎時間加上一開始輸入的增加時間，
當成第一個複製檔案的新修改時間，    
之後再依序複製檔案並隨機加上幾秒讓檔案時間看起來正常些。  
  

```CSharp
using System;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Reflection;
using System.Text.RegularExpressions;

namespace CompareList
{
    class Program
    {
        static string assemblyPath = Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
        static void Main(string[] args)
        {
            Console.WriteLine("請輸入增加時間(日(空格)時(空格)分):");
            //int addTime = 0;
            int addDays, addHours, addMins;

            //bool parsedSuccessfully = int.TryParse(Console.ReadLine(), out addTime);
            string input = Console.ReadLine();
            bool parsedSuccessfully = Regex.IsMatch(input, @"\d+\s\d+\s\d+");

            while (parsedSuccessfully == false)
            {
                Console.WriteLine("格式不符");
                Console.WriteLine("請輸入增加時間(日(空格)時(空格)分):");
                input = Console.ReadLine();
                //parsedSuccessfully = int.TryParse(Console.ReadLine(), out addTime);
                parsedSuccessfully = Regex.IsMatch(input, @"\d+\s\d+\s\d+");
            }

            string[] addTime = input.Split(new char[] { ' ' }, StringSplitOptions.RemoveEmptyEntries);
            addDays = int.Parse(addTime[0]);
            addHours = int.Parse(addTime[1]);
            addMins = int.Parse(addTime[2]);

            string compareList = Path.Combine(assemblyPath, "CompareList");
            if (!Directory.Exists(compareList))
            {
                Console.WriteLine("目標資料夾不存在!");
                Console.ReadKey();
            }
            else
            {
                //取案號
                string caseNO = new DirectoryInfo(assemblyPath).Name;

                //用cmd產生 附件三
                var psiNpm0 = new ProcessStartInfo
                {
                    FileName = "cmd",
                    RedirectStandardOutput = true,
                    RedirectStandardInput = true,
                    UseShellExecute = false
                };
                var pNpmRun0 = Process.Start(psiNpm0);
                pNpmRun0.StandardInput.WriteLine($@"DIR Update\SourceCode\*.* /S –D > 附件三-{caseNO}.txt");
                pNpmRun0.StandardInput.WriteLine("exit");
                pNpmRun0.WaitForExit();

                //用cmd產生 附件四-CompareList.txt
                var psiNpm1 = new ProcessStartInfo
                {
                    FileName = "cmd",
                    RedirectStandardOutput = true,
                    RedirectStandardInput = true,
                    UseShellExecute = false
                };
                var pNpmRun1 = Process.Start(psiNpm1);
                pNpmRun1.StandardInput.WriteLine(@"DIR CompareList\*.* /S –D > 附件四-CompareList.txt");
                pNpmRun1.StandardInput.WriteLine("exit");
                pNpmRun1.WaitForExit();

                DirectoryInfo d = new DirectoryInfo(compareList);
                FileInfo[] files = d.GetFiles("*.*"); //Getting files

                //新資料夾
                string newCompareList = Path.Combine(assemblyPath, "CompareListCheck");
                if (!Directory.Exists(newCompareList))
                    Directory.CreateDirectory(newCompareList);

                DateTime oTime = files.OrderBy(o => o.LastWriteTime).Last().LastWriteTime;   //基礎時間
                DateTime nTime = oTime.AddDays(addDays).AddHours(addHours).AddMinutes(addMins);
                Random rnd = new Random();
                //複製至另一資料夾並改變修改時間
                for (int i = 0; i < files.Length; i++)
                {
                    string copyFile = Path.Combine(newCompareList, files[i].Name);
                    File.Copy(files[i].FullName, copyFile, true);
                    nTime = nTime.AddSeconds(rnd.Next(15, 30));  //增加隨機秒數
                    File.SetLastWriteTime(copyFile, nTime);
                    File.SetCreationTime(copyFile, nTime);
                    File.SetLastAccessTime(copyFile, nTime);
                }

                //用cmd產生 附件四-CompareList.txt
                var psiNpm2 = new ProcessStartInfo
                {
                    FileName = "cmd",
                    RedirectStandardOutput = true,
                    RedirectStandardInput = true,
                    UseShellExecute = false
                };
                var pNpmRun2 = Process.Start(psiNpm2);
                pNpmRun2.StandardInput.WriteLine(@"DIR CompareListCheck\*.* /S –D > 附件四-CompareListCheck.txt");
                pNpmRun2.StandardInput.WriteLine("exit");
                pNpmRun2.WaitForExit();
            }

        }
    }
}
```

