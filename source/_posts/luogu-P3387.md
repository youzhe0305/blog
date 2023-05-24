---
title: <洛谷> P3387 【模板】縮點
top_img: transparent
date: 2023-05-24 10:31:02
tags:
    - 圖論
    - tarjan
    - 拓樸排序
    - 最長路
    - 普及+/提高
categories:
    - 洛谷
cover: /洛谷/luogu-P3387/108016482.jpg
description: <洛谷> P3387 【模板】縮點
---

## 題目: [P3387 【模板】縮點](https://www.luogu.com.cn/problem/P3387)

這算是tarjan-scc(找強連通分量的tarjan算法)的練習。題目中提到，路線可以重複走，不過每個點只能被計算一次，那顯然我們可以把一個可以從A點開始走，走回A點的子圖，也就是一個強連通分量，縮成一個點。因為要讓值最大，必定是把這個子圖裡面的點都走一遍，所以這個縮出來的點，點權就是這個子圖的所有點加總。

接著，得到縮完點的圖之後，會發現這個圖必定是DAG(有向無環圖)。因為每個點都是由一個強連通分量而來，如果兩個強連通分量之間，有環存在的話，他們兩個就會被併成一個強連通分量。

知道他是DAG後，我們就可以利用拓樸排序找最長路的方法，找到圖中權值最大的路徑。

順帶一提，這是tarjan的教學: [強連通分量 - OI Wiki](https://oi-wiki.org/graph/scc/)

```c++

#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1e4+5;

int n,m;
vector<int> gra[MAXN]; // 原圖
vector<int> gra_new[MAXN]; // 縮完點的圖
int w[MAXN]; // 點權

int DFN[MAXN];// tarjan-scc的DFN
int LOW[MAXN]; // tarjan-scc的LOW
vector<int> sta; // tarjan-scc的stack
int instack[MAXN]; // tarjan-scc的stack的instack
int vis[MAXN]; // 紀錄是tarjan否走過
int belong[MAXN]; // 記錄該點屬於那個縮點
vector<int> root; // 記錄縮出來的點有哪些(編號)
int ind=0; // tarjan-scc的順序

void tarjan(int v){ // 經典的tarjan算法

	DFN[v] = ++ind;
	LOW[v] = DFN[v];
	sta.push_back(v);
	instack[v] = 1;
	vis[v] = 1;

	for(int i:gra[v]){

		if(!vis[i]){
			tarjan(i);
			LOW[v] = min(LOW[v],LOW[i]);
		}
		else if(instack[i]){
			LOW[v] = min(LOW[v],DFN[i]);
		}
	}
	if(LOW[v] == DFN[v]){
		root.push_back(v);
		int total = 0; // 這個強連通分量中合起來的點權
		int u = sta.back();
		while(u!=v){
			belong[u] = v;
			sta.pop_back();
			instack[u] = 0;
			total += w[u]; // 加上每個點
			u = sta.back();
		}
		belong[u] = v; // 該點屬於以u為根的縮點
		sta.pop_back();
		instack[u] = 0;
		total += w[u];
		w[v] = total; // 讓這個代表縮點的根，點權變成整個子圖的總量

		return;
	}

}
int vis_2[MAXN]; // 紀錄DFS有沒有走過
int rd[MAXN]; // 入度
void DFS(int v){ // 用來建立新圖的DFS
	vis_2[v] = 1;
	for(int i:gra[v]){
        // 如果有不同縮點的點連接，相當於這兩個縮點連接
		if(belong[v]!=belong[i]) 
			gra_new[ belong[v] ].push_back(belong[i]), rd[belong[i]]++;
		if(!vis_2[i]){
			DFS(i);
		}
	}
}

int dp[MAXN]; // 拓樸最長路的DP
int topo(){ // 拓樸最長路

	int ans = 0;
	queue<int> que;
	for(int i:root){
		dp[i] = w[i];
		if(rd[i]==0) que.push(i),ans = max(ans,dp[i]);

	}
	while(!que.empty()){
		int u = que.front();
		que.pop();
		for(int i:gra_new[u]){
			rd[i]--;
			if(rd[i]==0) que.push(i);
			dp[i] = max(dp[i],w[i]+dp[u]);
			ans = max(ans,dp[i]);
		}
	}
	return ans;

}

signed main(){

cin>>n>>m;
for(int i=1;i<=n;i++)
	cin>>w[i];
for(int i=1;i<=m;i++){
	int a,b;
	cin>>a>>b;
	gra[a].push_back(b);
}

for(int i=1;i<=n;i++) // 用for迴圈避免1號點走不到的地方被忽略
	if(!DFN[i]) tarjan(i);

for(int i=1;i<=n;i++) // 用for迴圈避免1號點走不到的地方被忽略
	if(!vis_2[i]) DFS(i);

cout<<topo()<<endl;

}
// 測試用測資
/*

8 9
1 2 4 8 16 32 64 128
1 2
2 3
3 4
2 5
3 6
7 4
4 1
1 8
8 2

*/
// 輸出
/*
239
*/

```

封面圖來源: [メイド虹夏〜！](https://www.pixiv.net/artworks/108016482)

---