# JavaScript的日期


在整理筆記時，本已將材料備好準備大抄特抄。    
<!--more-->

稍微看一下準備怎麼寫時，才發現人家在文章尾端最後有介紹更棒的函式庫。  
> [Moment.js](https://momentjs.com/)
  
\
官網的演示範例  
=
> **Format Dates**  
moment().format('MMMM Do YYYY, h:mm:ss a'); // 四月 7日 2022, 9:27:58 上午  
moment().format('dddd');                    // 星期四  
moment().format("MMM Do YY");               // 4月 7日 22  
moment().format('YYYY [escaped] YYYY');     // 2022 escaped 2022  
moment().format();                          // 2022-04-07T09:27:58+08:00   
moment().format("hA");                          // 9AM   
  
> **Relative Time**  
moment("20111031", "YYYYMMDD").fromNow(); // 10 年前  
moment("20120620", "YYYYMMDD").fromNow(); // 10 年前  
moment().startOf('day').fromNow();        // 10 小時前  
moment().endOf('day').fromNow();          // 14 小時後  
moment().startOf('hour').fromNow();       // 31 分鐘前  
  
> **Calendar Time**  
moment().subtract(10, 'days').calendar(); // 2022/03/28  
moment().subtract(6, 'days').calendar();  // 上星期五 09:31  
moment().subtract(3, 'days').calendar();  // 上星期一 09:31  
moment().subtract(1, 'days').calendar();  // 昨天 09:31  
moment().calendar();                      // 今天 09:31  
moment().add(1, 'days').calendar();       // 明天 09:31  
moment().add(3, 'days').calendar();       // 下星期日 09:31  
moment().add(10, 'days').calendar();      // 2022/04/17  
  
> **Multiple Locale Support**  
moment.locale();         // zh-tw  
moment().format('LT');   // 09:32  
moment().format('LTS');  // 09:32:26  
moment().format('L');    // 2022/04/07  
moment().format('l');    // 2022/4/7  
moment().format('LL');   // 2022年4月7日  
moment().format('ll');   // 2022年4月7日  
moment().format('LLL');  // 2022年4月7日 09:32  
moment().format('lll');  // 2022年4月7日 09:32  
moment().format('LLLL'); // 2022年4月7日星期四 09:32  
moment().format('llll'); // 2022年4月7日星期四 09:32  
  
其他範例
=
>**自訂格式**  
moment().valueOf(); //1649295546752  
moment(1649295546752) // Thu Apr 07 2022 09:39:06 GMT+0800  
moment(1649295546752).format('YYYY-MM-DD') // 2022-04-07  
moment(1649295546752).format('YYYY-MM-DDTHH:mm:ss.SSS') // 2022-04-07T09:39:06.752  
  
>**Between**  
2018/1/1 是[2018, 0, 1]  月份從0開始
moment('要驗證的日期').isBetween('起始日', '截止日'); // true or false  
moment('2018-11-02').isBetween('2018-11-01', '2018-11-13'); // true  
moment('2010-10-20').isBefore('2010-10-19'); // false  
  
>**加減時間**  
moment().add(7, 'days'); // Thu Apr 14 2022 09:41:05 GMT+0800  
moment().add(7, 'days').add(1, 'months'); // Sat May 14 2022 09:41:33 GMT+0800  
  
>**時間差異**
var a = moment([2022, 4, 07, 12,0,0]);  
var b = moment([2022, 4, 06]);  
var diff = a.diff(b);  
var r = moment.duration(diff).asDays(); //1.5(轉換成天)   
var r = moment.duration(diff).asHours(); //36(轉換成小時)  
var r = moment.duration(diff).days(); //1(只取days部份)  
var r = moment.duration(diff).hours(); //12(只取hours部份)  

**取最大最小**  
```JavaScript
var friends = [{name: 'Angel', birthday: '11.12.1996'}, 
               {name: 'Eric' , birthday: '12.12.1989'}, 
               {name: 'Mark' , birthday: '5.01.1993'}]
var friendsBirthDays = friends.map(function(friend){
    return moment(friend.birthday, 'DD.MM.YYYY');
});
moment.max(friendsBirthDays).format('DD.MM.YYYY'); // 11.12.1996
```  
  
\
參考[官方文件](https://momentjs.com/docs/)可以看到更多應用。

