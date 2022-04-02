# INIFiles


之前開發都是使用app.config來放設定值。  
<!--more-->

但我覺得config用起來最大的問題是無用的資訊很多，太過雜亂。  
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <configSections>
        <sectionGroup name="userSettings" type="System.Configuration.UserSettingsGroup, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" >
            <section name="OC.Settings1" type="System.Configuration.ClientSettingsSection, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" allowExeDefinition="MachineToLocalUser" requirePermission="false" />
        </sectionGroup>
    </configSections>
    <userSettings>
        <OC.Settings1>
            <setting name="VBSname" serializeAs="String">
                <value>OC.bat</value>
            </setting>
            <setting name="connection" serializeAs="String">
                <value>Data Source=(local);Initial Catalog=Northwind;User Id=sa;Password=sa;</value>
            </setting>
            <setting name="waitTime" serializeAs="String">
                <value>3</value>
            </setting>
        </OC.Settings1>
    </userSettings>
</configuration>
```

後來發現了可以用ini檔存放設定，加上從接觸軟體就看過該檔案，  
覺得有為的資訊人員應該採用這種方法，所以就學了一下。  

在用過幾次以後又發現有大大寫出一個完整的class還附教學，  
那不拿來用真的太不好意思了。

```csharp
﻿/* ----------------------------------------------------------
* 
* 作者：qq450640526
* 
* 微信：roman_2015
* 时间: 2021年4月11日 14:54:43
* 更新：2021年9月21日 18:55:21
* 博客：https://www.cnblogs.com/xe2011/
*  
* 
* 说明
* [Conf]
* test = " A A SD F   "
* 分号的出现是为了保存值中左边或右边的空格的
* 
*  
*  模板使用utf-8格式 为了能支持特殊字符
* 
* 
------------------------------------------------------------ */

using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Reflection;
using System.Runtime.InteropServices;
using System.Text;
using System.Windows.Forms;
using static IniFiles.WinAPI;

namespace IniFiles {

    public class WinAPI {

        /*
       ------------------------------------------------------------------------------------------------------------- 
               BOOL WritePrivateProfileString(
                       LPCTSTR lpAppName,
                       LPCTSTR lpKeyName,
                       LPCTSTR lpString,
                       LPCTSTR lpFileName
               );

                其中各参数的意义
                       LPCTSTR lpAppName 是INI文件中的一个字段名.
                       LPCTSTR lpKeyName 是lpAppName下的一个键名,通俗讲就是变量名.
                       LPCTSTR lpString 是键值,也就是变量的值,不过必须为LPCTSTR型或CString型的.
                       LPCTSTR lpFileName 是完整的INI文件名，如果没有指定完整路径名，则会在windows目录（默认）查找文件。如果文件没有找到，则函数会在windows目录创建它。

       -------------------------------------------------------------------------------------------------------------
                UINT WINAPI GetPrivateProfileInt (
                   _In_LPCTSTR lpAppName, //The name of the sectionName in the initialization file.
                   _In_LPCTSTR lpKeyName, //The name of the key whose value is to be retrieved.
                   _In_INT nDefault, //The default value to return if the key name cannot be found in the initialization file.
                   _In_LPCTSTR lpFileName //The name of the initialization file
                   );

                   参数表编辑  
                       lpApplicationName String，指定在其中查找条目的小节。注意这个字串是不区分大小写的
                       lpKeyName String，欲获取的设置项或条目。这个支持不区分大小写
                       nDefault Long，指定条目未找到时返回的默认值
                       lpFileName String，初始化文件的名字。如果没有指定完整的路径名，windows就会在Windows目录中搜索文件

        -------------------------------------------------------------------------------------------------------------
            为一个初始化文件（.ini）中指定的小节设置所有项名和值
            返回值
            Long，非零表示成功，零表示失败。会设置GetLastError
           
            参数表
            WritePrivateProfileSection(
                LPCTSTR lpAppName, // 指向包含 sectionName 名称的字符串地址
                LPCTSTR lpString ,  // 要写入的数据的地址
                LPCTSTR lpFileName  // ini 文件的文件名
            );





       */
        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="keyName">LPCTSTR lpKeyName 是lpAppName下的一个键名,通俗讲就是变量名.</param>
        /// <param name="value">LPCTSTR lpString 是键值,也就是变量的值,不过必须为LPCTSTR型或CString型的.</param>
        /// <param name="fileName">LPCTSTR lpFileName 是完整的INI文件名，如果没有指定完整路径名，则会在windows目录（默认）查找文件。如果文件没有找到，则函数会在windows目录创建它。</param>
        /// <returns></returns>
        [DllImport("kernel32")]
        public static extern bool WritePrivateProfileString(string sectionName, string keyName, string value, string fileName);


        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName">LPCTSTR lpAppName 是INI文件中的一个字段名.</param>
        /// <param name="key">LPCTSTR lpKeyName 是lpAppName下的一个键名,通俗讲就是变量名</param>
        /// <param name="val"></param>
        /// <param name="fileName">LPCTSTR lpFileName 是完整的INI文件名，如果没有指定完整路径名，则会在windows目录（默认）查找文件。如果文件没有找到，则函数会在windows目录创建它。</param>
        /// <returns></returns>
        [DllImport("kernel32")]
        public static extern bool WritePrivateProfileString(byte[] sectionName, byte[] keyName, byte[] value, string fileName);

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="def"></param>
        /// <param name="retVal"></param>
        /// <param name="size"></param>
        /// <param name="fileName"></param>
        /// <returns></returns>
        [DllImport("kernel32")]
        public static extern int GetPrivateProfileString(byte[] sectionName, byte[] key, byte[] def, byte[] retVal, int size, string fileName);


        //需要调用GetPrivateProfileString的重载
        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="def"></param>
        /// <param name="retVal"></param>
        /// <param name="size"></param>
        /// <param name="fileName"></param>
        /// <returns></returns>
        [DllImport("kernel32")]
        public static extern long GetPrivateProfileString(string sectionName, string key, string def, StringBuilder retVal, int size, string fileName);

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="def"></param>
        /// <param name="retVal"></param>
        /// <param name="size"></param>
        /// <param name="fileName"></param>
        /// <returns></returns>
        [DllImport("kernel32")]
        public static extern uint GetPrivateProfileStringA(string sectionName, string key, string def, Byte[] retVal, int size, string fileName);

        /// <summary>
        /// 
        /// </summary>
        /// <param name="lpAppName"></param>
        /// <param name="lpKeyName"></param>
        /// <param name="nDefault"></param>
        /// <param name="lpFileName"></param>
        /// <returns></returns>
        [DllImport("kernel32")]
        public static extern int GetPrivateProfileInt(string lpAppName, string lpKeyName, int nDefault, string lpFileName);



        //获取ini文件所有的sectionName
        [DllImport("Kernel32.dll")]
        public extern static int GetPrivateProfileSectionNamesA(byte[] buffer, int iLen, string fileName);

        //获取指定sectionName的key和value
        [DllImport("Kernel32.dll")]
        public static extern int GetPrivateProfileSection(string lpAppName, byte[] lpReturnedString, int nSize, string lpFileName);

        /// <summary>
        /// 将指定的键值对写到指定的节点，如果已经存在则替换。
        /// </summary>
        /// <param name="SectionName">节点，如果不存在此节点，则创建此节点</param>
        /// <param name="Value">Item键值对，多个用\0分隔,形如key1=value1\0key2=value2
        /// <para>如果为string.Empty，则删除指定节点下的所有内容，保留节点</para>
        /// <para>如果为null，则删除指定节点下的所有内容，并且删除该节点</para>
        /// </param>
        /// <param name="iniFileName">INI文件</param>
        /// <returns>是否成功写入</returns>
        [DllImport("kernel32.dll", CharSet = CharSet.Auto)]
        public static extern bool WritePrivateProfileSection(string SectionName, string Value, string iniFileName);

        [DllImport("kernel32")]
        public static extern uint GetLastError();

    }

    public class IniFile {

        /// <summary>
        ///  <param name="iniFileName">ini的路径 请使用完整的路径"X:\test.ini"</param>
        /// </summary>
        public IniFile(string iniFileName) {
            //_iniFileName = iniFileName;

            _iniFileName = Path.GetFullPath(iniFileName);

            if (File.Exists(_iniFileName))
            {
                _iniFileEncoding = FileEncoding.GetEncoding(_iniFileName);


                #region 2021-9-21-15_32_03 当编码为 UTF-8 BOM，第1行如果有字段，则这个字段不能读取。

                try
                {

                    FileStream fs = new FileStream(iniFileName, FileMode.Open, FileAccess.Read, FileShare.ReadWrite);
                    StreamReader sr = new StreamReader(fs, _iniFileEncoding);

                    string firstLine = sr.ReadLine().Trim();

                    if (firstLine.Length >= 1)
                    {
                        if (firstLine.StartsWith("[") && firstLine.EndsWith("]"))
                        {
                            string msg = "\r\n\r\n";//";" + DateTime.Now.ToString() + "\tqq450640526提醒：当文本编码为 UTF-8 BOM，第1行如果有字段，则这个字段不能读取。  -- 软件自动添加。\r\n\r\n";
                            string str = File.ReadAllText(iniFileName, _iniFileEncoding);
                            File.WriteAllText(iniFileName, msg + str, _iniFileEncoding);
                        }
                    }
                }
                catch
                {
                        //**************异常文本 * *************
                        //System.NullReferenceException: 未将对象引用设置到对象的实例。
                        //在 IniFiles.IniFile..ctor(String fileName) 位置 E:\ASM\WindowsFormsApp1\IniFiles.cs:行号 83
                        //在 WindowsFormsApp1.Form1.btnQuery_Click(Object sender, EventArgs e) 位置 E:\ASM\WindowsFormsApp1\Form1.cs:行号 194
                        //在 System.Windows.Forms.Control.OnClick(EventArgs e)
                        //在 DevComponents.DotNetBar.ButtonX.OnClick(EventArgs e)
                        //在 DevComponents.DotNetBar.ButtonX.OnMouseUp(MouseEventArgs e)
                        //在 System.Windows.Forms.Control.WmMouseUp(Message & m, MouseButtons button, Int32 clicks)
                        //在 System.Windows.Forms.Control.WndProc(Message & m)
                        //在 System.Windows.Forms.NativeWindow.Callback(IntPtr hWnd, Int32 msg, IntPtr wparam, IntPtr lparam)
                }

                #endregion
            }
        }

        /// <summary>
        /// 在exe相对路径中自动产生 一个 配置文件
        /// 程序为 app1.exe 对应的配置 app1.exe.ini
        /// 
        /// 可以这样直接使用
        /// IniFile.Instance.WriteString("conf","name",textBox1.Text);
        /// textBox1.Text = IniFile.Instance.ReadString("conf","name");
        /// 在dll中获得调用当前DLL的路径
        /// 在DLL中用不了这样的写法 Application.ExecutablePath + ".ini";
        /// app1.exe 对应的 app1.exe.ini
        ///
        /// </summary>
        public static IniFile Instance => new IniFile(Assembly.GetEntryAssembly().Location + ".ini");
     
        /// <summary>
        /// 文件编码
        /// </summary>
        Encoding _iniFileEncoding = Encoding.Default;
        static string _iniFileName = "";

        /// <summary>
        /// 
        /// </summary>
        /// <param name="s"></param>
        /// <returns></returns>
        public static bool IsKeyContains(string s) {

            return false;
        }

        #region 核心部分



        private static byte[] GetBytes(string s) {
            return null == s ? null : Encoding.GetEncoding("utf-8").GetBytes(s);
        }

        //读取utf8编码格式的文件内容
        private string GetPrivateProfileString_UTF8(string sectionName, string key, string sValue, string _iniFileName) {
            int size = 65535;
            byte[] bt = new byte[size];
            int count = GetPrivateProfileString(GetBytes(sectionName), GetBytes(key), GetBytes(sValue), bt, size, _iniFileName);
            return Encoding.UTF8.GetString(bt, 0, count);
        }


        //写入utf8编码格式的文件内容
        private bool WriteStringUTF8(string sectionName, string key, string sValue, string fileName, string encodingName = "utf-8") {
            return WritePrivateProfileString(GetBytes(sectionName), GetBytes(key), GetBytes(sValue), fileName);
        }

        //读取信息
        private string Read(string sectionName, string key, string sValue = "") {
            //TestExecption(sectionName,key);
            if (_iniFileEncoding != Encoding.UTF8)
            {
                //ansi或gb2312格式的编码
                StringBuilder sb = new StringBuilder(4096);
                GetPrivateProfileString(sectionName, key, sValue, sb, 4096, _iniFileName);
                return sb.ToString();
            } else
            {
                return GetPrivateProfileString_UTF8(sectionName, key, sValue, _iniFileName);
            }
        }

        //写入信息
        private bool Write(string sectionName, string key, string sValue) {

            if (!IsSectionExists(sectionName))
            {
                /*
                新键的字段 前一行要是空行 这样文档看起来美观
                在文件末尾增加一个空行
                */
                string str = File.ReadAllText(_iniFileName, _iniFileEncoding);
                File.WriteAllText(_iniFileName,  str + "\r\n", _iniFileEncoding);
            }

            /*
             * 新增加的字段没有 换行符
             */
            //使用"" 能保存其中的空格字符
            if (_iniFileEncoding != Encoding.UTF8)
            {//ansi或gb2312格式的编码
                return WritePrivateProfileString(sectionName, key, " \"" + sValue + "\"", _iniFileName);
            } else
            {
                return WriteStringUTF8(sectionName, key, " \"" + sValue + "\"", _iniFileName);
            }
        }
        #endregion

        #region 字段、判断、测试异常

        /// <summary>
        /// 验证文件ini是否存在
        /// </summary>
        /// <returns>布尔值</returns>
        public bool ExistIniFile() {
            return File.Exists(_iniFileName);
        }


        //判断字段是否存在，字段和键名不区分大小写
        public bool IsSectionExists(string sectionName) {
            //全部转换成小写来判断 不区分大小写了
            string[] arr = ReadSections;

            for (int i = 0; i < arr.Length; i++)
            {
                arr[i] = arr[i].ToLower();
            }
            sectionName = sectionName.ToLower();

            return Array.IndexOf(arr, sectionName) > -1;
        }

        //检查某个sectionName下的某个键值是否存在，字段和键名不区分大小写
        public bool IsKeyExists(string sectionName, string key) {
            string[] arr = ReadKeyNames(sectionName);

            for (int i = 0; i < arr.Length; i++)
            {
                arr[i] = arr[i].ToLower();
            }

            key = key.ToLower();
            return Array.IndexOf(arr, key) > -1;
        }

        /// <summary>
        /// 字段或者键名不存在，尝试读取时抛出异常
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        private void TestExecption(string sectionName, string key) {
            if (!File.Exists(_iniFileName))
                throw new ApplicationException("文件不存在！ 读取数据失败！\n\"" + _iniFileName + "\"");
            else if (!IsSectionExists(sectionName))
            {

                throw new ApplicationException("字段[" + sectionName + "]不存在！\n路径: \"" + _iniFileName + "\"");

            } else if (!IsKeyExists(sectionName, key.ToLower()))
            {

                throw new ApplicationException("文件“" + _iniFileName + "”的\n字段[" + sectionName + "]的“" + key + "”键名，不存在！");
            }
        }


        //2021年5月1日 21:48:43发现不支持UTF-8
        /// <summary>
        /// 列出INI文件中所有的字段名  可以包含空的字段
        /// </summary>
        public string[] ReadSections {
            get { //ansi或gb2312格式的编码
                List<string> list = new List<string>();
                byte[] buf = new byte[65536];

                uint Length = GetPrivateProfileStringA(null, null, null, buf, buf.Length, _iniFileName);

                int j = 0;
                for (int i = 0; i < Length; i++)
                {
                    if (buf[i] == 0)
                    {                
                        if (_iniFileEncoding != Encoding.UTF8)
                            _iniFileEncoding = Encoding.UTF8;

                        list.Add(_iniFileEncoding.GetString(buf, j, i - j));
                        j = i + 1;
                    }
                }
                return list.ToArray();
            }
        }
        /// <summary>
        /// 读取当前ini所有的字段值 其中不含空的字段
        /// </summary>
        public string[] ReadSectionsWithoutWhiteSpace {
            get { //ansi或gb2312格式的编码
                List<string> list = new List<string>();
                byte[] buf = new byte[65536];

                uint Length = GetPrivateProfileStringA(null, null, null, buf, buf.Length, _iniFileName);

                int j = 0;
                for (int i = 0; i < Length; i++)
                {
                    if (buf[i] == 0)
                    {
                        if (_iniFileEncoding != Encoding.UTF8)
                            _iniFileEncoding = Encoding.UTF8;

                        string str = _iniFileEncoding.GetString(buf, j, i - j);
                        if (str.Trim() != "")
                            list.Add(str);

                        j = i + 1;
                    }
                }
                return list.ToArray();
            }
        }

        /// <summary>
        /// 读取字段下的所有键名,长度受限
        /// </summary>
        /// <param name="sectionName"></param>
        /// <returns></returns>
        public string[] ReadKeyNames(string sectionName) {

            if (_iniFileEncoding == Encoding.UTF8)
            {
                byte[] bt = Encoding.UTF8.GetBytes(sectionName);
                 sectionName = Encoding.Default.GetString(bt);
            }

            List<string> list = new List<string>();
            byte[] buf = new byte[9999999];
            uint len = GetPrivateProfileStringA(sectionName, null, null, buf, buf.Length, _iniFileName);
            int j = 0;
            for (int i = 0; i < len; i++)
            {
                if (buf[i] == 0)
                {
                    if (_iniFileEncoding == Encoding.UTF8)
                        list.Add(Encoding.UTF8.GetString(buf, j, i - j));
                    else
                        list.Add(Encoding.Default.GetString(buf, j, i - j));
                    j = i + 1;
                }
            }
            return list.ToArray();
        }

        /// <summary>
        /// 读取字段值的所有的键的值
        /// </summary>
        /// <param name="sectionName"></param>
        /// <returns></returns>
        public string[] ReadSectionValue(string sectionName) {
            List<string> result = new List<string>();
            foreach (string key in ReadKeyNames(sectionName))
                result.Add(Read(sectionName, key));

            return result.ToArray();
        }

        /// <summary>
        /// 清除某个sectionName 及其字段下所有的所有内容 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <returns></returns>
        public bool DeleteSection(string sectionName) {
            if (_iniFileEncoding == Encoding.UTF8)
            {
                byte[] bt = Encoding.UTF8.GetBytes(sectionName);
                sectionName = Encoding.Default.GetString(bt);
            }
            bool b = WritePrivateProfileString(sectionName, null, null, _iniFileName);
            if (!b)
                throw (new ApplicationException("无法清除Ini文件中的sectionName"));

            return b;
        }

       /*
        * 通过把文件读取到STRING[]然后一行一行来判断
        */
        /// <summary>
        /// 修改节点名称
        /// </summary>
        /// <param name="section_old"></param>
        /// <param name="section_replaceStr"></param>
        /// <returns></returns>
        public bool WriteSection(string section_old, string section_replaceStr) {
            try
            {
                //修改ini节点字段
                string[] arr = File.ReadAllLines(_iniFileName, _iniFileEncoding);
                if (arr.Length > 0) ;
                {
                    if (arr[0].Trim().StartsWith("[") && arr[0].Trim().EndsWith("]"))
                    {
                        arr[0] = "\t";
                    }
                }
                for (int i = 1; i < arr.Length; i++)
                {
                    if (arr[i].Trim() == "[" + section_old + "]")
                        arr[i] = "[" + section_replaceStr + "]";
                }

                File.WriteAllLines(_iniFileName, arr);//保存文本
                return true;
            }
            catch(Exception ex)
            {
                MessageBox.Show(ex.Message);
                return false;
            }
          
        }



        /// <summary>
        /// 修改字段名
        /// </summary>
        /// <param name="section_old">原字段名</param>
        /// <param name="section_new">新字段名</param>
        /// <param name="value"></param>
        /// <returns></returns>

        public bool EditSection(string section_old, string section_new) {
            //return WritePrivateProfileSection(sectionName, value, _iniFileName);

            string[] keys = ReadKeyNames(section_old);
            string[] keyValues = ReadSectionValue(section_old);

            //MessageBox.Show(string.Join("\r\n", keyValues));

            DeleteSection(section_old);

            for (int i = 0; i < keys.Length; i++)
                WriteString(section_new, keys[i], keyValues[i]);


            return false;
            /*
             1 读取当前字段值
             2 读取当前字段所有的属性名和属性值
             3 删除当前字段
             4 重新写入 新字段 旧属性

            此方法的BUG会 修改字段到文本最后的位置

             */
        }
      

    /// <summary>
    /// 删除某个sectionName下的键 及其对应的值
    /// </summary>
    /// <param name="sectionName"></param>
    /// <param name="key"></param>
    /// <returns></returns>
    public bool DeleteKey(string sectionName, string key) {
            if (_iniFileEncoding == Encoding.UTF8)
            {
                byte[] bt = Encoding.UTF8.GetBytes(sectionName);
                sectionName = Encoding.Default.GetString(bt);
            }
            return WritePrivateProfileString(sectionName, key, null, _iniFileName);
        }


        #endregion

        #region 读和写 string、Boolean、Integer、Single、Double、StringArray

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public string ReadString(string sectionName, string key, string sValue = "") {
            return Read(sectionName, key, sValue);
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public bool WriteString(string sectionName, string key, string sValue) {
            return Write(sectionName, key, sValue);
        }

        //返回值为true false 不区分大小写
        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public bool ReadBoolean(string sectionName, string key, bool sValue = false) {
            string v = sValue.ToString().ToLower();
            string s = Read(sectionName, key, v).ToLower().Trim();

            //必须要返回一个值
            return Convert.ToBoolean(s);
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public bool WriteBoolean(string sectionName, string key, bool sValue) {
            return Write(sectionName, key, sValue.ToString().Trim().ToLower());
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public int ReadInteger(string sectionName, string key, int sValue = 0) {
            string v = sValue.ToString().ToLower();
            string s = Read(sectionName, key, v).Trim();
            return Convert.ToInt32(s);
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public bool WriteInteger(string sectionName, string key, int sValue) {
            return Write(sectionName, key, sValue.ToString().Trim().ToLower());
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public double ReadDouble(string sectionName, string key, double sValue = 0d) {
            string v = sValue.ToString().ToLower();
            string s = Read(sectionName, key, v).ToLower().Trim();
            return Convert.ToDouble(s);
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public bool WriteDouble(string sectionName, string key, double sValue) {
            return Write(sectionName, key, sValue.ToString().Trim().ToLower());
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public double ReadSingle(string sectionName, string key, double sValue = 0d) {
            string v = sValue.ToString().ToLower();
            string s = Read(sectionName, key, v).ToLower().Trim();
            return Convert.ToSingle(s);
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="key"></param>
        /// <param name="sValue"></param>
        /// <returns></returns>
        public bool WriteSingle(string sectionName, string key, double sValue) {
            return WriteDouble(sectionName, key, sValue);
        }

        //2014年6月24日19:39:25
        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <returns></returns>
        public string[] ReadStringArray(string sectionName) {
            int len = ReadInteger(sectionName, "Count", 0);
            List<string> list = new List<string>();
            for (int i = 0; i < len; i++)
            {
                string s = ReadString(sectionName, i.ToString(), "0");
                list.Add(s);
            }
            return list.ToArray();
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <returns></returns>
        public string ReadStringArrayText(string sectionName) {
            return string.Join("\n", ReadStringArray(sectionName));
        }


        /*  为了确保能正确显示空格请使用单引号 或者双引号将值加入进去
            磁的时候不用解析直接读出来的
            [TEST]
            a = '  asd a as  '
            b = ' a a '
        */
        /// <summary>
        /// 
        /// </summary>
        /// <param name="sectionName"></param>
        /// <param name="stringArray"></param>
        public void WriteStringArray(string sectionName, string[] stringArray) {
            DeleteSection(sectionName);
            WriteInteger(sectionName, "Count", stringArray.Length);
            for (int i = 0; i < stringArray.Length; i++)
                WriteString(sectionName, i.ToString(), stringArray[i]);

        }

        #endregion
    }


    class FileEncoding {
        /// <summary> 
        /// 给定文件的路径，读取文件的二进制数据，判断文件的编码类型 
        /// </summary> 
        /// <param name="path">文件路径</param> 
        /// <returns>文件的编码类型</returns> 
        public static Encoding GetEncoding(string path) {
            if (File.Exists(path))
            {
                FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Read);
                Encoding r = GetType(fs);
                fs.Close();
                return r;
            } else
                return null;
        }

        /// <summary> 
        /// 通过给定的文件流，判断文件的编码类型 
        /// </summary> 
        /// <param name="fs">文件流</param> 
        /// <returns>文件的编码类型</returns> 
        private static Encoding GetType(FileStream fs) {
            byte[] Unicode = new byte[] { 0xFF, 0xFE, 0x41 };
            byte[] UnicodeBIG = new byte[] { 0xFE, 0xFF, 0x00 };
            byte[] UTF8 = new byte[] { 0xEF, 0xBB, 0xBF }; //带BOM 
            Encoding reVal = Encoding.Default;

            BinaryReader r = new BinaryReader(fs, System.Text.Encoding.Default);
            int i;
            int.TryParse(fs.Length.ToString(), out i);
            byte[] ss = r.ReadBytes(i);
            if (IsUTF8Bytes(ss) || (ss[0] == 0xEF && ss[1] == 0xBB && ss[2] == 0xBF))
            {
                reVal = Encoding.UTF8;
            } else if (ss[0] == 0xFE && ss[1] == 0xFF && ss[2] == 0x00)
            {
                reVal = Encoding.BigEndianUnicode;
            } else if (ss[0] == 0xFF && ss[1] == 0xFE && ss[2] == 0x41)
            {
                reVal = Encoding.Unicode;
            }
            r.Close();
            return reVal;

        }

        /// <summary> 
        /// 判断是否是不带 BOM 的 UTF8 格式 
        /// </summary> 
        /// <param name="data"></param> 
        /// <returns></returns> 
        private static bool IsUTF8Bytes(byte[] data) {
            int charByteCounter = 1;
            //计算当前正分析的字符应还有的字节数 
            byte curByte; //当前分析的字节. 
            for (int i = 0; i < data.Length; i++)
            {
                curByte = data[i];
                if (charByteCounter == 1)
                {
                    if (curByte >= 0x80)
                    {
                        //判断当前 
                        while (((curByte <<= 1) & 0x80) != 0)
                        {
                            charByteCounter++;
                        }
                        //标记位首位若为非0 则至少以2个1开始 如:110XXXXX...........1111110X　 
                        if (charByteCounter == 1 || charByteCounter > 6)
                        {
                            return false;
                        }
                    }
                } else
                {
                    //若是UTF-8 此时第一位必须为1 
                    if ((curByte & 0xC0) != 0x80)
                    {
                        return false;
                    }
                    charByteCounter--;
                }
            }
            if (charByteCounter > 1)
            {
                throw new Exception("非预期的byte格式");
            }
            return true;
        }
    }
}
```
這個class包山包海，我還沒用過全部的功能。  
而且不知是他有寫錯，還是我的問題，用其中一個method時有發生錯誤。 
但忘了在哪遇到，有機會再看看。  


使用說明
======

## 引用
using IniFiles;

## 實例化
IniFile ini = new IniFile(" c:\app.ini");

yourInipath 可以是如下2種方法
和EXE在同一目錄的路徑： app.ini
完整的文件路徑： c:\app.ini

## 讀取內容
ini文件結構
[user]
name = "測試"
age = "14"
phone = "12345678910"
sex = "true"

[note]
Count= "3"
0= "【推薦】了解你才能更懂你，博客園首發問卷調查，助力社區新升級"
1= "【推薦】超50萬行VC++源碼: 大型組態工控、電力仿真CAD與GIS源碼庫"
2= "【推薦】獨家首發 | 900頁阿里文娛技術實戰，8大技術棧解析技術全景"


## 讀取數據
string s =  ini.ReadString("user","name"); //返回 測試
int age = ini.ReadString("user","age"); //14
bool b = ini.ReadBoolean("user","sex"); //讀出來的值總是 小寫的  true或false

## 讀取多行內容
textBox1.Lines =  ini.ReadStringArrayText("note");

## 列出ini文件中所有的字段名
string[] sections = ini.SectionNames;

## 讀取[user]字段下的所有鍵名
string[] keys= ini.ReadKeyNames("user");

## 讀取[user]字段下的所有鍵名的值
string[] values= ini.ReadSectionKeyValues("user");


## 寫入數據
  ini.WriteString("user","name"，”test”);
  ini.WriteInteger("user","age",18); 
  ini.WriteBoolean("user","sex",true); 

## 可以將多行文本保存到INI中
  ini.WriteStringArray("note",textBox1.Lines);

## 讀取異常
如果.ini文件不存在、或者目標字段、或者目標鍵值不存在則拋出異常。

## 靜態讀寫
使用 IniFile.Instance.方法名
如果使用此種寫法則 配置文件默認和exe在同一目錄，假如程序為 app1.exe則配置文件為app1.exe.ini

### 讀
   textBox1.Text = IniFile.Instance.ReadString("conf","name");
### 寫
   IniFile.Instance.WriteString("conf","name",textBox1.Text);


