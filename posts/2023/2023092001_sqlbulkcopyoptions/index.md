# SqlBulkCopyOptions

比較常用的有兩個：
<!--more-->

1. KeepIdentity：保留來源的PK值，否則會出現複製過去的資料產生標識列發現變化的情況！  
2. KeepNulls： 資料為空時不填入預設值。  
<br>
  
其他屬性如下表

| 屬性 | 作用 |
| ----------- | ----------- |
| CheckConstraints|	在插入資料時檢查條件約束。 根據預設，不會檢查條件約束。 |
|Default	|使用所有選項的預設值。|
FireTriggers|	若已指定，則會導致此伺服器對於正在插入至此資料庫的資料列，引發插入觸發程序。|
KeepIdentity|	保留來源識別值。 如果未指定，則識別值依目的地指派。|
KeepNulls|	不論預設值的設定為何，均保留目的地資料表中的 null值。 如果未指定，則null值會以適用的預設值取代。|
TableLock|	在大量複製作業期間，取得大量更新鎖定。 如果未指定，則會使用資料列鎖定。|
UseInternalTransaction|	若已指定，則大量複製作業的每個批次將在交易內發生。 如果指定這個選項，同時也提供 SqlTransaction 物件給建構函式，則 ArgumentException 就會發生。|
