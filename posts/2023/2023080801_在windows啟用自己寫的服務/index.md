# 在Windows啟用自己寫的服務

啟用服務不難，難的是自訂服務名稱。
<!--more-->

安裝Service(無法自訂名稱)
```cmd
CD C:\Windows\Microsoft.NET\Framework\v4.0.30319\

InstallUtil.exe /servicename="DTS_PushService" /DisplayName="DTS_PushService" "D:\Push_DTSService\DTSService.exe"
```
<br>

解除安裝Service
```cmd
CD C:\Windows\Microsoft.NET\Framework\v4.0.30319\

InstallUtil.exe /u /servicename="ABOT.GateWayMonitor(HCE).Service" "C:\Program Files (x86)\ABOT (IVR)\ABOT.GateWayMonitor.Service.Setup\ABOT.GateWayMonitor.Service.exe"
```
<br>

安裝Service(無法自訂名稱)
```cmd
CD C:\Windows\Microsoft.NET\Framework\v4.0.30319\

InstallUtil.exe /u /servicename="ABTDataTransferService" "D:\Push_DTSService\DTSService.exe"
```
<br>

最後用`PowerShell`才搞定
```cmd
New-Service -Name "DTS_PushService" -BinaryPathName "D:\Push_DTSService\DTSService.exe"

Remove-Service -Name "DTS_PushService"
```
