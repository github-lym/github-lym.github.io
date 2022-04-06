# NPOI直接存取Excel


之前Vincent讓我接了一個case。  

<!--more-->
  
這個case一點都不難，就是整理一Excel資料，  
把內容分門別類並填上相關字詞至原本的Excel。  
並沒有任何需要技術的地方。  
\
\
**但我還是做到很度爛。**  
\
\
原因是它有期限，我只能利用上班的空檔做。  
但打開那個表密密麻麻，內容又動輒上百字，很容易眼花兼疲憊。  
\
剛好那時很常接觸NPOI，就打算寫個[程式](https://github.com/github-lym/V_Excel)來處理。  
[![執行畫面](demo.webp)](demo.webp)  
\
我當時筆電記憶體才8G，直接存取Excel執行起來會頓，  
所以最初是採用將Excel匯入SQLite，修改完後再匯出。  
\
後來做完後連此程式也交了出去，負責的人沒工具匯入匯出，  
所以我又改回NPOI直接存取。  
\
採用了[ComboBox的模糊查詢](../comboboxfuzzysearch)，  
還有按方向鍵移至上下一筆。  
```CSharp
        protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
        {
            int seq = 1;
            if (keyData == Keys.Up)
            {
                if (!string.Empty.Equals(TB_SEQ.Text))
                {
                    int.TryParse(TB_SEQ.Text, out seq);
                    seq--;
                    if (seq < 1)
                        seq = 1;
                    BTN_CLEAR_Click(null, null);
                    TB_SEQ.Text = seq.ToString();
                    BTN_QUERY_Click(null, null);
                    return true;
                }
            }
            //capture down arrow key
            if (keyData == Keys.Down)
            {
                if (!string.Empty.Equals(TB_SEQ.Text))
                {
                    int.TryParse(TB_SEQ.Text, out seq);
                    seq++;
                    BTN_CLEAR_Click(null, null);
                    TB_SEQ.Text = seq.ToString();
                    BTN_QUERY_Click(null, null);
                    return true;
                }
            }
            return base.ProcessCmdKey(ref msg, keyData);
        }
```
