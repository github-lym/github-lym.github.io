# 檢查服務是否執行

測試機的服務常自動關閉，所以找來這方法。
<!--more-->
  
```
@Echo Off
Set ServiceName=Jenkins


SC queryex "ABOT.ATMPGateway.Service"|Find "STATE"|Find /v "RUNNING">Nul&&(
    echo ABOT.ATMPGateway.Service not running 
    echo Start ABOT.ATMPGateway.Service

    Net start ABOT.ATMPGateway.Service>nul||(
        Echo ABOT.ATMPGateway.Service wont start 
        exit /b 1
    )
    echo ABOT.ATMPGateway.Service started
    exit /b 0
)||(
    echo ABOT.ATMPGateway.Service working
    exit /b 0
)
```
  
<br>

做成一批次檔，每10分鐘檢查一次，目前看來可行。

