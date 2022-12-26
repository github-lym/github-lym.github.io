# 正規表示法筆記

有時遇到一些正規表示法的問題，做個筆記記錄一下。  
<!--more-->
  
******
#### Email檢查  
`^\w+((-\w+)|(\.\w+))*\@[A-Za-z0-9]+((\.|-)[A-Za-z0-9]+)*\.[A-Za-z]+$`  
> ^\w+：@ 之前必須以一個以上的文字&數字開頭，例如 abc  
> ((-\w+)：@ 之前可以出現 1 個以上的文字、數字或「-」的組合，例如 -abc-  
> (\.\w+))：@ 之前可以出現 1 個以上的文字、數字或「.」的組合，例如 .abc.  
> ((-\w+)|(\.\w+))*：以上兩個規則以 or 的關係出現，並且出現 0 次以上 (所以不能 –. 同時出現)  
> @：中間一定要出現一個 @  
> [A-Za-z0-9]+：@ 之後出現 1 個以上的大小寫英文及數字的組合  
> (\.|-)：@ 之後只能出現「.」或是「-」，但這兩個字元不能連續時出現  
> ((\.|-)[A-Za-z0-9]+)*：@ 之後出現 0 個以上的「.」或是「-」配上大小寫英文及數字的組合  
> \.[A-Za-z]+$/：@ 之後出現 1 個以上的「.」配上大小寫英文及數字的組合，結尾需為大小寫英文  
******
    
******  
#### 取得XML某tag裡的值  
`/(?<=<TOTA>[^]+\blength=")[^"]+(?="[^]+<\/TOTA>)/g`  
這個在[regex101](https://regex101.com)的`ECMAScript`模式是可行的。 
但我在VSCode裡用，它認不出`[^]`(各種字元與換行的字)。  
改用`(?<=<TOTA>(.|\n)+\blength=")[^"]+(?="(.|\n)+<\/TOTA>)`就沒問題了。  
  
> 前面是`<TOTA>(避免抓到<TITA>)裡length`除了`"`以外的值，  
> 值後面是`"加上</TOTA>`。  

改用`(?<=<TOTA>(.|\n)+\blength=")\d+(?="(.|\n)+<\/TOTA>)`是一樣的效果。  
下面是想取得的內容範例。 
```XML
<TxDef encoding="EBCDIC" txAdapter="Unisys" appAdapter="pass" xmlTransformer="Unisys" transportAdapter="SystemF" targetTx="" txMapper="" delimiter="" memo="pp">
  <TITA>
      <TxBlock dataTag="" renderTag="Y" memo="" ref="SystemF-TITA-COM-AREA" />
      <TxField id="BCURCD" cname="cc" datatype="9" lengthtype="F" padchar=" " justify="" default="" length="2" lengthExpr="" scale="0" tagSize="0" lengthSize="0" encoding="" shiftInOut="Y" invisibleChar="TrimAndPadRight" memo="" optional="N" overwrite="N" codec="" renderTag="" charFormat="" />
      <TxField id="STATUS" cname="c6" datatype="9" lengthtype="F" padchar=" " justify="" default="" length="1" lengthExpr="" scale="0" tagSize="0" lengthSize="0" encoding="" shiftInOut="Y" invisibleChar="TrimAndPadRight" memo="" optional="N" overwrite="N" codec="" renderTag="" charFormat="" />
      <TxField id="END" cname="gg" datatype="X" lengthtype="F" padchar=" " justify="" default="" length="1" lengthExpr="" scale="0" tagSize="0" lengthSize="0" encoding="" shiftInOut="Y" invisibleChar="TrimAndPadRight" memo="" optional="N" overwrite="N" codec="" renderTag="" charFormat="" />
    </TxBody>
    <TxTail dataTag="" renderTag="Y" memo="" ref="" />
  </TITA>
  <TOTA>
    <TxHead dataTag="" renderTag="Y" memo="" ref="" />
    <TxBody dataTag="CommMsg" renderTag="Y" memo="" ref="">
      <TxRepeat dataTag="TXREC" renderTag="Y" memo="" timesField="" timesValue="-1" name="">
        <TxBlock dataTag="Header" renderTag="N" memo="" ref="SystemF-TOTA-BASIC-TxBlock" />
        <TxSwitch dataTag="" renderTag="N" memo="" switchField="WARN">
          <TxCase dataTag="" renderTag="N" memo="" value="[default]">
            <TxRepeat dataTag="TXREC" renderTag="N" memo="" timesField="" timesValue="1" name="">
              <TxField id="CNAME" cname="ed" datatype="X" lengthtype="F" padchar=" " justify="" default="" length="80" lengthExpr="" scale="0" tagSize="0" lengthSize="0" encoding="UNISYS" shiftInOut="Y" invisibleChar="TrimAndPadRight" memo="" optional="N" overwrite="N" codec="" renderTag="" charFormat="" />
              <TxField id="AVBAL" cname="b7" datatype="S" lengthtype="F" padchar=" " justify="" default="" length="14" lengthExpr="" scale="2" tagSize="0" lengthSize="0" encoding="" shiftInOut="Y" invisibleChar="TrimAndPadRight" memo="" optional="N" overwrite="N" codec="" renderTag="" charFormat="" />
              <TxField id="AVG03" cname="hh" datatype="9" lengthtype="F" padchar=" " justify="" default="" length="13" lengthExpr="" scale="2" tagSize="0" lengthSize="0" encoding="" shiftInOut="Y" invisibleChar="TrimAndPadRight" memo="" optional="N" overwrite="N" codec="" renderTag="" charFormat="" />
              <TxField id="AVG1" cname="1m" datatype="9" lengthtype="F" padchar=" " justify="" default="" length="13" lengthExpr="" scale="2" tagSize="0" lengthSize="0" encoding="" shiftInOut="Y" invisibleChar="TrimAndPadRight" memo="" optional="N" overwrite="N" codec="" renderTag="" charFormat="" />
               </TxRepeat>
            </TxRepeat>
          </TxCase >
        </TxSwitch >
      </TxRepeat >
    </TxBody>
    <TxTail dataTag="" renderTag="Y" memo="" ref="" />
  </TOTA>
</TxDef>
```
******
  
******
#### 取得有重複字的句子  
`/\b(\S+) (\1)\b/gi`  
> \1是重複第一組(group)的意思，最後加上i是不分大小寫。  
******
  
******
#### 必須包含至少一位大寫英文、一位小寫英文、一位數字  
`^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).+$`  
> 開頭必須有1個以上的大小寫英文及數字的組合  
  
本來看這個一頭霧水，後來不斷在網路上尋找相似案例和測試才略懂略懂。  
  
[![範例](regex01.webp '範例')](regex01.webp)  
圖上的範例是我找到的。  
`(?=\w{6})(?=\d{2})`取得後方是六個字，且有兩個數字。  
那只有`12bana`、`123456`符合兩數字開頭跟有六字的條件。  
`bana12`因非數字開頭就不符合。  
\
同理，如果把`^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).+$`  
留下三個lookahead條件，  
變為`^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)`。  
[![範例](regex02.webp '範例')](regex02.webp)  
就表示後面要接一位大寫英文、一位小寫英文、一位數字為條件就比較好理解了。
******
