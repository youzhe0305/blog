---
title: <洛谷> P2184 貪婪大陸
top_img: transparent
date: 2023-07-18 01:13:48
tags:
    - 資料結構
    - 區間
    - BIT
    - 提高+/省選-
categories:
    - 洛谷
cover: /洛谷/luogu-P2184/103594107.jpg
description: <洛谷> P2184 貪婪大陸
---

## 題目: [P2184 貪婪大陸](https://www.luogu.com.cn/problem/P2184)

一開始看到這題，我一直在想要怎麼同時維護區間最大值，又能夠去做增減的修改，但一直想不到解法。

但是圖畫出來後，會發現一件很酷的事情，這題的本質是一個區間裡面包了幾個區間，而區間與區間之間會有6個情況，針對6個情況去找共通點跟差異點就可以處理了。

6種情況如下:
![](/洛谷/luogu-P2184/interval.png)


會增加區間中地雷種類的有: 部分在左、完全在內、部分在右、包住整個區間

可以發現他們的共通點是: 左端點全部在要求區間右端點的左側

不過這個特性也包含到了 完全在左，所以要找出完全在左跟這4個的差異性，也就是右端點在要求區間左端點的左側。

所以用(要求區間右端點左側的左端點) - (要求區間左端點左側的右端點)就可以求得會增加地雷種類的區間有幾個。

那這裡我們只要開兩個BIT分別維護左端點的區間值跟右端點的區間值，就可以解掉這題

```c++
#include<iostream>
#define lowbit(x) x&-x
using namespace std;

const int MAXN = 1e5+5;
int bit_left[MAXN]; // 維護左端點的BIT
int bit_right[MAXN]; // 維護右端點的BIT
int n,m;

// 基本的BIT作法
void modify(int bit[], int x, int val){ 
    while(x<=n){
        bit[x] += val;
        x += lowbit(x);
    }
}

int query(int bit[], int x){
    int ans = 0;
    while(x>0){
        ans += bit[x];
        x -= lowbit(x);
    }
    return ans;
}

signed main(){

    cin>>n>>m;

    while(m--){

        int q,l,r;
        cin>>q>>l>>r;

        if(q==1){
            modify(bit_left, l, 1); // 維護左端點值
            modify(bit_right, r, 1); // 維護右端點值
        }
        if(q==2){
            int ans = query(bit_left, r) - query(bit_right, l-1); // (要求區間右端點左側的左端點) - (要求區間左端點左側的右端點)
            cout<<ans<<endl;
        }
    }
}
```

封面圖來源: [虹夏ちゃん](https://www.pixiv.net/artworks/103594107)

---