# 在Visual Studio 2015上使用Fody


Fody是個好用的套件，可以把多個dll檔跟執行檔合併成單一檔案。  
<!--more-->

公司的還在用Visual Studio 2015，裝Fody方法不對就會有不相容情況。 
  
**甚至整個專案變成無法編譯！**
  
  
找了半天終於找到解決這問題的文章。

## 安裝Fody  
從套件管理器主控臺中輸入Install-Package Fody -Version 4.2.1來安裝4.2.1版本的Fody。  
  
## 安裝Costura.Fody
套件管理器主控臺中輸入Install-Package Costura.Fody -Version 3.3.3來安裝3.3.3版本的Costura.Fody。  
  
## 安裝完成重新建置
  
## 若有錯誤可修改FodyWeavers.xml
錯誤提示如下：  
Fody: No configuration entry found for the installed weaver Costura. This weaver will be skipped. You may want to add this weaver to your FodyWeavers.xml  
  
修改FodyWeavers.xml內容為如下：  
```xml
<?xml version="1.0" encoding="utf-8"?>
<Weavers xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="FodyWeavers.xsd">
  <Costura />
</Weavers>
```
  
照著做終於解決了相容性問題！
  
