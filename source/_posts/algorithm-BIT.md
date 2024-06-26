---
title: <演算法筆記> BIT資料結構
top_img: transparent
date: 2023-07-18 00:45:28
tags:
    - 資料結構
    - 區間
    - BIT
categories:
    - 演算法筆記
cover: img/cover/algorithm.jpg
description: 講解關於BIT資料結構的概念與實作
---

BIT(Binary Indexed Tree)，樹狀數組，用來維護區間的資料結構，一般情況下，我們會用它處理單點修改區間查詢的區間加值問題，當然他也可以處理更複雜的問題，不過就要搭配差分等，其他的資料結構，這裡暫且不談。

BIT的核心概念是倍增，每個區間都可以使用2的次方長度的子區間構成，所以我們只要維護2的次方長度的區間。舉例來說，7的二進制是1101，也就是他可以用1~4、5~6、7三個區間組成。

我們實際需要維護的區間如下圖，以每個數字的格子作為尾端，做一個區間，區間的長度是該數字轉成二進制後，最右邊的1代表的值，像是6=110，最右邊的1是在第2位，也就是2的1次方。
![](/演算法教學/algorithm-BIT/BIT.png)


至於為什麼會這樣，是因為我們每前進一個數字，就會從二進制的最右端+1，這時候可能會發生進位，但不論如何進位，該數字都會在最右端的區間裡，所以取最右端的1當作區間長度，再更左的1，就用這個數字之前的區間構成。

這裡我們定義x轉成二進制後，最右端的1為lowbit(x)，在數學上的計算方法中，剛好會是(x&-x)的值，至於為什麼我就不知道了。

BIT與線段樹的主要差別就是，他的空間只需要N，因為存的是以每個數字當尾端的區間值，所以區間值的數量剛好會跟數字一樣多，也就是N，比線段樹的4N小。另外，BIT的常數也比線段樹小一點。

接下來講講BIT的具體實現方法。

### 區間查詢

如上所提到的，一個從1開始的區間可以變成二進制的底下，所有1代表的區間構成，所以我們就要一一找出對應的區間。

假設我們要找1~6，該數字6會在最右邊的區間，也就是lowbit(6)，所以我們先把答案加上這個區間的值，然後數字減去這個區間的長度，以6為例，就是減掉5~6的區間長2，變成4，換成二進制就是110去掉最右邊的1，變成100，這樣4又變成了lowbit(4)的尾端了，這樣重複做下去，直到0就可以獲得1~x的區間值。

當我們想要得到，l~r的區間值時，只要用前綴和的方法。1~r減去1~(l-1)，就可以了。

複雜度: $O(logN)$
```c++
#define lowbit(x) x&-x

int query(int bit[], int x){
    int ans = 0;
    while(x>0){
        ans += bit[x];
        x -= lowbit(x);
    }
    return ans;
}

```

### 單點修改

單點修改想起來就比較抽象了，當我們修改其中一個點時，我們就要連帶修改所有會被影響到的區間，而這些區間，從BIT圖可以看出，就是以這個點所在的區間開始，向上找父區間(包含到該區間的區間)。

從BIT圖中，可以看出，只要加上該點的區間長度，就會變成他往上一個的父區間，也就是加上lowbit(x)。往上的父區間出現變動時，一樣加上lowbit，就可以找到更上面被影響到的區間。

複雜度: $O(logN)$
```c++
void modify(int bit[], int x, int val){ // 直接用加的比較好寫
    while(x<=n){
        bit[x] += val;
        x += lowbit(x);
    }
}
```

### 建立BIT

既然我們掌握了單點修改的方法，只要對每個點做修改，就可以做到初始的建立了，複雜度: $O(NlogN)$


---