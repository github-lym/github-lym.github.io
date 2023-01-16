# 版更程式

用來版更的懶人程式。
<!--more-->
```cmd
@Echo Off
Set "sd=D:\EAI\APServer\txDef\SystemF"
Set "dd=D:\換版備份"
Set "ex1=C:\inetpub\CMS\WebApi\Logs"
:Set "ex2=D:\Angular\Sample"
:RoboCopy "%sd%" "%dd%\%ds%" /MIR /S /E /Z /ZB /R:5 /W:5 /TBD /NP /V /MT:32

set /p caseno="輸入案號=>"


for /f "tokens=2 delims==" %%a in ('wmic OS Get localdatetime /value') do set "dt=%%a"
set "YY=%dt:~2,2%" & set "YYYY=%dt:~0,4%" & set "MM=%dt:~4,2%" & set "_DD=%dt:~6,2%"
set "HH=%dt:~8,2%" & set "Min=%dt:~10,2%" & set "Sec=%dt:~12,2%"
set "datestamp=%YYYY%%MM%%_DD%" & set "timestamp=%HH%%Min%%Sec%" & set "fullstamp=%YYYY%-%MM%-%_DD%_%HH%%Min%-%Sec%"


RoboCopy "%sd%" "%dd%\%datestamp%\SystemF" /MIR /S /E /R:5 /W:5 /TBD /NP /NFL /V /MT:32 


pause

cd D:\EAI\
attrib -a /s D:\EAI\*.*

pause

RoboCopy "D:\_Update\Svr(61)\EAI" "D:\EAI" /S /E /R:5 /W:5 /TBD /NP /NFL /V /MT:32 

pause

dir D:\EAI\\*.* /aa /s > D:\_Update\%caseno%_EAI_61_OBJ.TXT


iisreset /RESTART

pause
```
