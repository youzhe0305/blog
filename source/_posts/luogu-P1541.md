---
title: <洛谷> P1541 [NOIP2010 提高組] 烏龜棋
top_img: transparent
date: 2023-06-13 17:47:29
tags:
    - dp
categories:
    - 洛谷
    - 普及+/提高
cover: /洛谷/luogu-P1541/107025668.jpg
description: <洛谷> P1541 [NOIP2010 提高組] 烏龜棋
---

## 題目: [P1541 [NOIP2010 提高組] 烏龜棋](https://www.luogu.com.cn/problem/P1541)

這題很顯然是個標準的DP，我一開始的想法是，直接開一個5維的陣列$dp[格子數][卡片1][卡片2][卡片3][卡片4]$，枚舉格子數去做線性dp，但這樣會直接RE。所以勢必要把**格子數**去掉，陣列才可能放得下，後來發現格子數其實就是使用的卡片數量乘以權值加總，所以就可以直接丟掉格子數，改成枚舉4種卡片的數量。

轉移式變成以下:
使用卡片1:

$dp[卡片1][卡片2][卡片3][卡片4] = dp[卡片1 - 1][卡片2][卡片3][卡片4] + arr[step]$

arr是儲存格子權值的陣列，step是當前的位置，也就是卡片數量乘以權值加總。
其他以此類推...

```c++

#include<bits/stdc++.h>
#define int long long
using namespace std;

const int MAXN = 355;
const int MAXM = 45; // 40^4 = 2.5e6 是可以接受的空間複雜度
int arr[MAXN]; // 儲存格子權值
int card[5]; // 儲存卡片數量
int dp[MAXM][MAXM][MAXM][MAXM];

signed main(){

int n,m;
cin>>n>>m;

for(int i=1;i<=n;i++){
    cin>>arr[i];
}

for(int i=1;i<=m;i++){
    int a;
    cin>>a;
    card[a]++;
}

dp[0][0][0][0] = arr[1]; // 初始站在起點，把起點的值加進來
for(int i=0;i<=card[1];i++)
    for(int j=0;j<=card[2];j++)
        for(int k=0;k<=card[3];k++)
            for(int l=0;l<=card[4];l++){
                int step = 1 + i*1 + j*2 + k*3 + l*4; // 起始點是1，所以用完當下這些卡片會到的格子要+1
                if(i>0)
                    dp[i][j][k][l] = max(dp[i][j][k][l], dp[i-1][j][k][l] + arr[step]);
                if(j>0)
                    dp[i][j][k][l] = max(dp[i][j][k][l], dp[i][j-1][k][l] + arr[step]);
                if(k>0)
                    dp[i][j][k][l] = max(dp[i][j][k][l], dp[i][j][k-1][l] + arr[step]);
                if(l>0)
                    dp[i][j][k][l] = max(dp[i][j][k][l], dp[i][j][k][l-1] + arr[step]);
            }

cout<<dp[ card[1] ][ card[2] ][ card[3] ][ card[4] ]<<endl;

}

```

封面圖來源: [イメチェン꒰ঌ ( ˶'ᵕ'˶) ໒꒱｢に、似合うかな....｣](https://www.pixiv.net/artworks/107025668)

---