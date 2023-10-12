---
title: 放火直播被攻擊的手法
top_img: transparent
date: 2023-10-04 16:13:59
tags:
    - 雜談
    - 資安
    - Unicode
categories:
    - 雜談
cover: 雜談/just-talk-LouisStreamAttack/103963751.jpg
description: 放火直播被攻擊的手法解析
---

[影片](https://www.youtube.com/watch?v=gKD3tSSlta0)

這個影片裡有提到，在留言刷空白會導致留言的功能崩潰。

其中攻擊者有提到，他刷的東西不是空白，而是看似空白，實則裡面有100*10000多字的字串。只是這東西在傳送出去的時候，看起來就是空白的樣子。

首先，把一坨字壓在一個幾乎看不見的格子裡，而不是直接傳送大量空白，是因為playone的直播平台是不允許只傳空白的。但如果傳一堆看的見的字，就很容易被看出來ban掉。所以攻擊者使用把字全部壓在一起的手法，讓一般人看不出來留言其實會造成大量的伺服器負擔。

這個方式的原理來自於unicode本身的特性，unicode裡面，有些東西是不占任何格子的，像是[附加符號](https://zh.wikipedia.org/zh-tw/%E7%B5%84%E5%90%88%E9%99%84%E5%8A%A0%E7%AC%A6%E8%99%9F)(◌̀)之類的，因為他是疊在另一個字上的，所以不占任何空間，當把一堆附加符號疊在一起時，還是長得跟只疊一個一樣。

示例如下，兩個點占了10000多字:
![](/雜談/just-talk-LouisStreamAttack/word_num_cal.png)

在直播平台只會顯示這個(這個包了40字):
![](/雜談/just-talk-LouisStreamAttack/attack_picture.png)

不過這個東西只是看起來跟一個一樣而已，如果平台有做字數計算，就可以看出這個東西其實占了超大的空間

所以只要透過python來生成這個unicode疊起來的東西，就可以製造這個超大空間的字了

程式碼示例如下:
```python
# 用來把數字轉成unicode的函式，有興趣請查到Unicode的編碼
def NumToUnicode(num): # num type: string 
    if(len(num)>4):
        return "Wrong Unicode value"
    while(len(num)<4):
        num = '0'+num
    unicode_str = f'\\u{num}'
    return (unicode_str.encode().decode("unicode_escape")) * 10000 # 影片中攻擊者用更大的數字，這裡只用1萬示範
    # decode, encode請參考https://hackmd.io/@9UfXRWPzS62YaxdR7uwZ0Q/B1K4TRr16
    # unicode_escape是用來反轉帶有\u的unicode編碼的字串形式

if __name__ == "__main__":
    explosion = NumToUnicode("0358") # 0358是一個附加符號的unicode碼，有興趣請自己查表
    print(f"{explosion}") # 輸出出來的字，點兩下整個複製就是那個佔位超大，但看起來只有一個的字
    # 如果直播間沒有留言字數限制，可以直接複製他拿去發，服務器就會當機了
```

封面圖來源: [虹夏ちゃん](https://www.pixiv.net/artworks/103963751)


---


