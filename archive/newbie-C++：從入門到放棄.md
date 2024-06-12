---
title: <新手教學題> C++：從入門到放棄
top_img: transparent
date: 2023-07-17 20:20:33
tags: 
    - 新手教學題
categories:
    - 新手教學題
cover: /新手教學題/newbie-C++：從入門到放棄/103727545.jpg
description: <新手教學題> C++：從入門到放棄
---

## 題目: [C++：從入門到放棄](http://mdcpp.mingdao.edu.tw/problem/T042)

這題也是迴圈的應用，首先，我們可以看出，每複製一次行數會變成2倍。並且有兩種情況，要求的行數剛好是2的次方以及不是的，剛好是2的次方，則一直複製全部，直到相等就停止。不是2的次方的情況，一樣複製全部，複製到超過他的那一次時，會發現我們多複製了一些，事實上只要這次不要複製那麼多，指複製需要的，那也會在這一次滿足。

所以說，2的次方=>等於時停止，非2的次方=>大於時停止，所以只要行數小於(非等於、大於就是小於了)要求時，就一直複製全部，每複製一次，增加一次次數，就可以算出答案。

```c++

#include<iostream>
using namespace std;

signed main(){

    int t;
    cin>>t; // 有t組詢問
    while(t--){
        int a;
        cin>>a; // 要求的數字
        int num = 1; // 行數
        int ans = 0; // 複製次數
        while(num<a){ // 承上敘述，小於時，就一直複製
            num *= 2; // 複製成兩倍
            ans++; // 每複製一次，次數+1
        }
        cout<<ans<<endl;
    }
}

```

封面圖來源: [虹夏ちゃんと](https://www.pixiv.net/artworks/106379296)

---