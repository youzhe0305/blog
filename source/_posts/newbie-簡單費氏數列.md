---
title: <新手教學題> 簡單費氏數列
top_img: transparent
date: 2023-07-17 20:48:16
tags: 
    - 新手教學題
categories:
    - 新手教學題
cover: /新手教學題/newbie-簡單費氏數列/104730590.jpg
---

## 題目: [簡單費氏數列](http://mdcpp.mingdao.edu.tw/problem/B001)

這題不用看題目，直接看標題就行，就是要找費氏數列，不知道費氏數列是什麼的可以google，費式數列的每一項都是前二項相加，所以我們可以用for迴圈直接推導到第30項(N<=30)，並用陣列儲存答案，再來他問哪一項我們就答哪一項

```c++

#include<iostream>
using namespace std;

signed main(){

    int arr[35]; // 費式數列的值，題目只要求N<=30，所以這裡開35
    int T;
    cin>>T; // 詢問次數
    arr[1] = 1; // 初始值
    arr[2] = 2; // 初始值
    for(int i=3;i<=30;i++){
         arr[i] = arr[i-1] + arr[i-2]; // 費式數列的算法: 前二項相加=這一項
    }

    while(T--){
        int a;
        cin>>a; // 問第a項
        cout<<arr[a]<<endl; // 回答第a項的答案
    }
}

```

封面圖來源:[虹夏ちゃん](https://www.pixiv.net/artworks/104730590)

---