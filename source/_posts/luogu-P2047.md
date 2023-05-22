---
title: <洛谷> P2047 [NOI2007] 社交網路
top_img: transparent
date: 2023-05-22 13:30:00
tags: 
    - 最短路
    - Floyd Warshall
    - 普及+/提高
categories:
    - 洛谷
cover: /洛谷/luogu-P2047/108188130.jpg
description: <洛谷> P2047 [NOI2007] 社交網路

---

## 題目: [P2047 [NOI2007] 社交網路](https://www.luogu.com.cn/problem/P2047)

首先，題目要我們求出，一個點的重要性，而這個重要性會從每條經過他的最短路徑加總而來，所以可以推測這題大概是多點源最短路。另外$N<=100$，所以Floyd Warshall演算法的$N^3$時間複雜度是可以接受的。所以我們利用Floyd Warshall演算法來處理。

接著，從題目可以看出，我們需要兩個東西:
1. i到j的最短路有幾個
2. i到j的最短路中，經過k的有幾個

先假設i到j的最短路數量是path[i][j]

第一項可以透過Floyd Warshall進行更新時處理，當出現更短的路徑時，代表path[i][j]刷新。因為Floyd Warshall是在經過第k個點被鬆弛時更新，也就是會在k點的兩側各挑一條最短路，因此新的最短路數量就是path[i][k] * path[k][j](就基本的乘法原理)。

第二項，要確定某條最短路有沒有經過k，只要判斷path[i][k] + path[k][j]會不會等於path[i][j]，如果等於的話就一定是，大於顯然不是，也不會有小於的狀況，不然就出現更短的路徑了。而path[i][k] * path[k][j]就會是經過k的最短路數量。

最後在把兩者相除，即個得到k對於一組(i,j)的重要性，接著遍歷一遍即可

```c++
#include <bits/stdc++.h>
#define int long long
#define pii pair<int,int>
#define F first
#define S second
using namespace std;

const int MAXN = 105;
vector<pii> vec[MAXN]; // 圖
int dp[MAXN][MAXN]; // 最短距離
double path[MAXN][MAXN]; // 最短路徑數量
double imp[MAXN]; // 重要性

signed main(){

int n,m;

cin>>n>>m;

for(int i=1;i<=n;i++)
for(int j=1;j<=n;j++){
	dp[i][j] = 10000000005;
	path[i][j]=1; // 這樣更新的時候才有值，不然就變成0*0
	if(i==j) dp[i][j] = 0;
}
for(int k=1;k<=n;k++){
	imp[k] = 0;
}

for(int i=1;i<=m;i++){
	int a,b,w;
	cin>>a>>b>>w;
	vec[a].push_back({b,w});
	dp[a][b] = w;
	vec[b].push_back({a,w});
	dp[b][a] = w;
}



for(int k=1;k<=n;k++)
	for(int i=1;i<=n;i++)
		for(int j=1;j<=n;j++){
			if(i==k || j==k) continue; // 避免出現用自己來更新，結果重複累加的狀況
			if(dp[i][j] > dp[i][k] + dp[k][j]){
				dp[i][j] = dp[i][k] + dp[k][j];
				path[i][j] = path[i][k] * path[k][j]; // 因為是新的最短距離，所以重新給path一個值
			}
			else if(dp[i][j] == dp[i][k] + dp[k][j]){
				path[i][j] += path[i][k] * path[k][j]; // 因為是同個最距離，所以不重新給，用累加
			}
		}
for(int k=1;k<=n;k++){
	for(int i=1;i<=n;i++)
		for(int j=1;j<=n;j++){
			if(i==k || j==k) continue; // 避免出現把自己更新自己記入的情況
			if(dp[i][k] + dp[k][j] == dp[i][j]){ // 表示此最短路必定經k
				if(path[i][j]==0) continue;
				imp[k] += path[i][k] * path[k][j] / path[i][j];
			}

		}
	cout<<fixed<<setprecision(3)<<imp[k]<<endl;
}

}
```

封面圖來源: [メイド虹歌ちゃん](https://www.pixiv.net/artworks/108188130)

---
