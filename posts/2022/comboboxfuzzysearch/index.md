# ComboBox的模糊查詢


要不是Vincent，我可能還沒機會做這個。    
<!--more-->

```CSharp
 private void comboBox1_TextUpdate(object sender, EventArgs e)
        {
            //清空combobox
            this.comboBox1.Items.Clear();

            //清空listNew
            listNew.Clear();

            //開始查所有候選資料
            foreach (var item in listOnit)
            {
                if (item.Contains(this.comboBox1.Text))
                {
                    //符合，插入ListNew
                    listNew.Add(item);
                }
            }
            //combobox添加已經查到的關鍵詞
            this.comboBox1.Items.AddRange(listNew.ToArray());
            //設置選擇位置，否則選擇位置始終保持在第一列，造成輸入關鍵詞的倒序排列
            this.comboBox1.SelectionStart = this.comboBox1.Text.Length;
            //保持游標原來狀態，有時候游標指針會被下拉框覆蓋，所以要進行一次設置。
            Cursor = Cursors.Default;
            //自動彈出下拉框
            this.comboBox1.DroppedDown = true;
        }
```

然後combobox的事件TextUpdate選上面這個function。

