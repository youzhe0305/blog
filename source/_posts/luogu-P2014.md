---
title: <洛谷> P2014 [CTSC1997] 選課
top_img: transparent
date: 2023-05-23 10:28:26
tags:
    - 圖論
    - DP
    - 樹
    - 普及+/提高
categories:
    - 洛谷
cover: /洛谷/luogu-P2014/108353513.jpg
description: <洛谷> P2014 [CTSC1997] 選課
---

## 題目: [P2014 [CTSC1997] 選課](https://www.luogu.com.cn/problem/P2014)

這種選課的題目，我第一個想法是DAG，不過後來想了想，用DAG來求最大值好像不太好做。所以就改成我們常常用來求最大值的方法---DP

這題要進行DP首先要解決的問題是，他有多的入度為0的點，有就是說起點有好幾個，這樣是不能直接做DP的，所以這裡要用到一個技巧，我們設置一個超級原點(編號0)，連到那些入度為0的點，再去做後續的DP，就會好做很多。

並且當我們把超級原點加進去之後，會發現整個圖形變成了一棵樹，這時候就很明顯的要使用樹DP了。

至於轉移式的處理，這裡的轉移式不是單純的一個式子，而更像是背包DP那樣的轉移。

核心概念是，我們先把大問題簡化，假設有棵樹，根結點底下有x個子樹，我們已知關於這個子樹的所有數據，現在我們要做的是，決定從各個子樹裡面，要各自挑多少堂課，才會組成最大值。這裡就像是背包問題，只是從挑選要或不要，變成挑選各要拿幾個。

所以我們會試著找，這棵樹裡面我們總共要拿w堂課時，也就對應到背包問題的重量。然後枚舉子樹裡面要拿幾堂課(j)可以得到最大值，這樣就算是完成這個背包問題裡面的第一個物品(子樹)。

可以寫出轉移式:
$$
    dp[v][w] = max(dp[v][w],dp[v][w-j]+dp[sub][j])
$$

接著枚舉總共拿幾堂課的不同情況(不同w)，也就對應到背包問題的枚舉重量，得到挑到目前的子樹時，不同課數下的最大值。

然後換到下一個物品，繼續做同樣的枚舉，最後就可以得到，這棵樹中，拿多少堂課的最大值。

知道轉移式之後，我們只要照一般樹DP的作法，DFS到底部，再遞迴回來，就可以在我們的超級原點，得到整個圖來講，拿多少堂課可以得到的最大值。

```c++

#include <bits/stdc++.h>
using namespace std;

const int MAXN = 305;
vector<int> tree[MAXN]; // 樹
int score[MAXN]; // 紀錄這個點的分數
int n,m;
int dp[MAXN][MAXN]; // 點編號,選的課數

void DFS(int v){
	if(tree[v].empty()) return; // 到葉節點直接return
    
	for(int sub:tree[v]){ // 枚舉子樹(物品)
		DFS(sub); // 樹DP的基本作法，DFS下去
        // 接著做類似背包的轉移
		for(int i=m;i>=1;i--){ // 枚舉要拿幾堂課(必定包含自己，所以至少1)
			for(int j=0;j<=i-1;j++){ // 枚舉這個子樹裡，要拿多少課最好
				dp[v][i] = max(dp[v][i], dp[v][i-j] + dp[sub][j]); 
			}
		}
	}

}

signed main(){


cin>>n>>m;

for(int i=1;i<=n;i++){
	int k,s;
	cin>>k>>s;
	score[i] = s;
	dp[i][1] = s; // 初始化，在點i當根節點的時候，只拿一個必定是自己
	tree[k].push_back(i); 
    // 在這裡有k=0是沒有入度的情況
    // 所以我們直接把超級原點編號設0，就可以不用特殊處理
}

m++; // 因為會多選到一個超級原點，所以要多加一個課程量

DFS(0); // 從超級原點開始
int ans=0;

for(int i=0;i<=m;i++){
	ans = max(ans,dp[0][i]);
}

cout<<ans<<endl;

}

```

封面圖來源: [早めの夏!!暑すぎます☀️](https://www.pixiv.net/artworks/108353513)

---