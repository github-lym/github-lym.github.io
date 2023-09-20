# 在本機除錯Service

接手一WindowsServie，但除錯得裝一些額外的東西。
<!--more-->

想說能不能懶惰一點，結果找到以下方法。  
1. 先把輸出類型改為主控台應用程式。  
[![檔案屬性](../img/2023092002_01.png '檔案屬性')](../img/2023092002_01.png)  
<br>
2. 在啟動那邊增加Debug區塊  
```CSharp
#define DEBUG
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using System.ServiceProcess;
using System.Text;
using System.Threading.Tasks;

namespace DTSService
{
    static class Program
    {
        /// <summary>
        /// 應用程式的主要進入點。
        /// </summary>
        static void Main()
        {
            //TODO 打開 上板時

            ServiceBase[] ServicesToRun;
            ServicesToRun = new ServiceBase[]
            {
                new DTSService()
            };
            ServiceBase.Run(ServicesToRun);


            //------------------------------------------
            //TODO 關閉
            //StandAlone 本機測試
            //DTSService ss = new DTSService();
            //ss.test();
            //ss.DoTransfer();
            //------------------------------------------

#if (DEBUG)
            // 01-於VS中偵錯時請先註解此部分
            // ServiceBase.Run(ServicesToRun);

            // 02-使用RunInteractive來執行原有Service功能以進行偵錯
            RunInteractive(ServicesToRun);
#endif
        }

#if (DEBUG)
        static void RunInteractive(ServiceBase[] servicesToRun)
        {
            // 利用Reflection取得非公開之 OnStart() 方法資訊
            MethodInfo onStartMethod = typeof(ServiceBase).GetMethod("OnStart",
                BindingFlags.Instance | BindingFlags.NonPublic);

            // 執行 OnStart 方法
            foreach (ServiceBase service in servicesToRun)
            {
                Console.Write("Starting {0}...", service.ServiceName);
                onStartMethod.Invoke(service, new object[] { new string[] { } });
                Console.Write("Started");
            }

            Console.WriteLine("Press any key to stop the services");
            Console.ReadKey();

            // 利用Reflection取得非公開之 OnStop() 方法資訊
            MethodInfo onStopMethod = typeof(ServiceBase).GetMethod("OnStop",
                BindingFlags.Instance | BindingFlags.NonPublic);

            // 執行 OnStop 方法
            foreach (ServiceBase service in servicesToRun)
            {
                Console.Write("Stopping {0}...", service.ServiceName);
                onStopMethod.Invoke(service, null);
                Console.WriteLine("Stopped");
            }
        }
#endif

    }
}
```  

<br>
雖然不懂原理是啥，但能用就先筆記。
