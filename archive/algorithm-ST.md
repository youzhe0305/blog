---
title: <演算法筆記> ST表資料結構
top_img: transparent
date: 2023-07-17 17:03:21
tags:
    - 資料結構
    - 區間
    - ST表
categories:
    - 演算法筆記
cover: img/cover/algorithm.jpg
description: 講解關於ST表的概念與實作
---

ST表，Sparse-Table演算法，用來解決可重複貢獻的區間問題，也就是求大區間時，可以用兩個重疊的小區間獲得(通常就是用來解區間最大最小值，區間加值因為重疊會累加，所以不行)

ST表使用了倍增的方式，先建好所有2的次方數長度的區間，然後把任意區間用兩個長度大於區間一半的子區間去構成，如圖:

![](/演算法教學/algorithm-ST/st-preprocess-lift.svg)
(圖取自:[OI wiki](https://oi-wiki.org/ds/sparse-table/))

利用兩個長度4的區間，直接算出長度6的區間的最大值，而這兩個長度4的區間，會在我們ST表裡面(2的次方)。

這樣的算法中，因為我們以每個點為起點，建構2的次方長度的區間，建構的複雜度為$O(NlogN)$，在查詢時，只要兩個ST表的值取極值，所以複雜度是$O(1)$，這也是他比線段樹優勢的地方，雖然差不太多就是了。

實作如下:

令ST表為，$ST[i][j]$，$i$代表起點，$j$代表長度為2^j次方。
![](/演算法教學/algorithm-ST/ST-len-interval.png)
初始化讓$ST[i][0]$為$i$位置的值，然後用下列式子轉移
$ST[i][j] = max(ST[i][j-1],ST[i+2^{j-1}][j-1])$
也就是把一個$2^j$長度的區間，用兩個$2^{j-1}$長度的區間構成

接著就可以開始查詢了，一個區間的數值，會是兩個區間之間去找，這兩個區間的長度要是第一個大於等於區間一半長度的2次方，並分別從頭尾延伸向中間。
假設最左為$l$最右為$r$，子區間長度為$2^s$，轉移式如下:
$ans = max(ST[l][s],ST[r-2^s+1][s])$


## 示範code:
```c++
#include<bits/stdc++.h>
#define int long long
using namespace std;

const int MAXN = 1e5+5;
const int LOGN = log2(MAXN)+1; // 最大的log值
int arr[MAXN]; // 儲存資料
int logn[MAXN]; // 某個數字的log值向下取整，也就是大於區間一半的第一個2的次方數
int st[MAXN][LOGN]; // ST表
int n,m; // n:資料數量, m:詢問數量

void pre_log(){ // 預處理log值
    logn[1] = 0;
    logn[2] = 1;
    for(int i=3;i<=MAXN;i++)
        logn[i] = logn[i/2] + 1; // 這個整數小於真正的log值，但不會差到1，所以可以覆蓋到一半以上
}

void init(){ // 初始化ST表
    for(int i=1;i<=n;i++)
        st[i][0] = arr[i];
}


signed main(){

    cin>>n>>m;
    for(int i=1;i<=n;i++)
        cin>>arr[i];

    pre_log();
    init();

    for(int j=1;j<=LOGN;j++){ // 枚舉不同長度的區間，直到最大
        for(int i=1;i + (1<<j) - 1 <=n;i++) // st表的儲存範圍 = (i,i+2^j - 1)(長度2^j)
            st[i][j] = max( st[i][j-1], st[i + (1<<(j-1))][j-1] ); // 用兩個長度2^(j-1)的區間構成
    }

    while(m--){ // m次
        int l,r; // 區間的左右端點
        cin>>l>>r;
        int len = r-l+1; // 區間長度
        int s = logn[len]; // 涵蓋一半的2的次方數
        int ans = max(st[l][s],st[ r - (1<<s) +1 ][s]);
        cout<<ans<<endl;
    }
}

```
注意: <<的位移符號，依然遵循由左到右計算，並沒有優先，所以要括號

---