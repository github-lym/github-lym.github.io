# WinForm抓取鍵盤輸入


做個紀錄。

<!--more-->

```CSharp
protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
	//capture up arrow key
	if (keyData == Keys.Up)
	{
		MessageBox.Show("You pressed Up arrow key");
		return true;
	}
	//capture down arrow key
	if (keyData == Keys.Down)
	{
		MessageBox.Show("You pressed Down arrow key");
		return true;
	}
	//capture left arrow key
	if (keyData == Keys.Left)
	{
		MessageBox.Show("You pressed Left arrow key");
		return true;
	}
	//capture right arrow key
	if (keyData == Keys.Right)
	{
		MessageBox.Show("You pressed Right arrow key");
		return true;
	}
	return base.ProcessCmdKey(ref msg, keyData);
}

```

