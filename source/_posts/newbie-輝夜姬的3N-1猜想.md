---
title: <新手教學題> 輝夜姬的3N+1猜想
top_img: transparent
date: 2023-07-11 19:40:53
tags: 
    - 新手教學題
categories:
    - 新手教學題
cover: /新手教學題/newbie-輝夜姬的3N-1猜想/EfFr0P9.jpg
description: <新手教學題> 輝夜姬的3N+1猜想
---

## 題目: [輝夜姬的3N+1猜想](http://mdcpp.mingdao.edu.tw/problem/T001)

這題是while迴圈的應用，因為我們不確定他會在甚麼時候才完成操作，所以必須使用while迴圈，在達成條件，X==1之前，不斷重複操作。

在迴圈運行時，我們要預先開一個變數，用來計算迴圈跑的次數，也就是操作次數，迴圈每跑一次就+1，等迴圈結束，所得的值就是X變成1的操作次數。

順帶一提，這裡用到了while(T--)的寫法，這代表跑T次的意思，因為當T次跑完，T就會減至0，也就是false的意思，會導致迴圈停止，所以迴圈就會剛好只跑T次

```c++
#include<iostream>
using namespace std;

int main(){

    int T;
    cin>>T;

    while(T--){ // 這部分請看解說第3段
        long long X; // 題目要求用long long存
        cin>>X;

        int num = 0; // 操作次數
        while(X!=1){ // 不斷迴圈，直到X達到條件
            num++; // 每跑一次，操作次數+1
            if(X%2==0) // 偶數操作
                X = X/2;
            else // 奇數操作
                X = X*3+1;
        }
        cout<<num<<endl;
    }
}
```

---