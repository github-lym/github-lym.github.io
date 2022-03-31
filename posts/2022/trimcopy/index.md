# 去掉文字前後空白


做客服工程師那會，每天都要查詢資料庫。  

<!--more-->
但資料庫有個欄位當初設計為char而非varchar，  
複製這欄位做之後的查詢還得去除掉後面的空白造成困擾。  
\
當時的想法是如果可以在剪貼簿上把前後空白刪掉就好了。 
但即便對現在的我來說仍是有困難的，還好有得抄。   

找到類似的稍微改一下達到[我要的](https://github.com/github-lym/TrimCopy)，  
加上可以將程式縮到工作列與點擊切換啟動與否。  
```csharp
        //hide form from Alt-Tab dialog
        protected override CreateParams CreateParams
        {
            get
            {
                // Turn on WS_EX_TOOLWINDOW style bit
                CreateParams cp = base.CreateParams;
                cp.ExStyle |= 0x80;
                return cp;
            }
        }

        //form invisible
        protected override void OnVisibleChanged(EventArgs e)
        {
            base.OnVisibleChanged(e);
            this.Visible = false;
        }

        public Form1()
        {
            InitializeComponent();
            nextClipboardViewer = (IntPtr)SetClipboardViewer((int)this.Handle);
            //this.ShowInTaskbar = false; //不顯示在底下工具列(改設定在form屬性)
            this.Hide();    //隱藏視窗
            this.notifyIcon1.ContextMenu = new ContextMenu();
            this.notifyIcon1.ContextMenu.MenuItems.Add(new MenuItem("EXIT",  new EventHandler(Exit)));
            notifyIcon1.Icon = Properties.Resources.checkIcon;
        }
```
```csharp
        private void notifyIcon1_MouseClick(object sender, MouseEventArgs e)
        {
            if (trimWorking == true)
            {
                trimWorking = false;
                notifyIcon1.Icon = Properties.Resources.unCheckIcon;
                notifyIcon1.ShowBalloonTip(100, null, "Stop Working", ToolTipIcon.Warning);
            }
            else if (trimWorking == false)
            {
                trimWorking = true;
                notifyIcon1.Icon = Properties.Resources.checkIcon;
                notifyIcon1.ShowBalloonTip(100, null, "Start Working", ToolTipIcon.Warning);
            }    
        }

        private void Exit(object sender, EventArgs e)
        {
            this.Close();
        }
```
雖然偶爾會跳出異常，不過方便多了。  
\
如今回過頭來看，還真的是有些地方可以改改看看還會不會跳出異常。

