---
title: <新手教學題> 大富翁
top_img: transparent
date: 2023-07-18 09:51:03
tags: 
    - 新手教學題
categories:
    - 新手教學題
cover: /新手教學題/newbie-大富翁/106953959.jpg
description: <新手教學題> 大富翁
---

這一題的核心，就是模擬大富翁的走路，首先我們要先用陣列儲存所有格子上的數字。接著開一個變數step代表目前所在的格子，從0開始，也就是從第0格開始。再開一個初始值為0的變數cost，代表加起來的過路費。然後開始累加骰子的數值，每累加一次，也就是走一步，就要把過路費加到cost上面。

```c++
#include<iostream>
using namespace std;

signed main(){

    int n,m;
    int arr[10005];

    cin>>n>>m;

    for(int i=1;i<=n;i++)
        cin>>arr[i]; // 儲存每一格過路費的值

    int step = 0; // 所在的格子數，題目有說從第0格開始
    int cost = 0; // 所花的過路費

    while(m--){ // 做m次
        int b;
        cin>>b;
        step += b; // 往前走b格
        cost += arr[step]; // 加上腳下這個格子的過路費
    }
    cout<<cost<<endl;
}

```

封面圖來源: [虹夏ちゃん](https://www.pixiv.net/artworks/106953959)

---