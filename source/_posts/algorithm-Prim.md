---
title: <演算法筆記> Prim最小生成樹演算法
top_img: transparent
date: 2023-05-29 14:27:13
tags:
    - 圖論
    - 最小生成樹
    - Prim
categories:
    - 演算法筆記
cover: img/cover/algorithm.jpg
description: 講解關於Prim最小生成樹演算法的概念與實作
---

# 臨時打字用

Prim，普林演算法，用類似Dijkstra的方式，找出一張圖裡面，能連到所有邊的樹中，權值最小的，也就是[最小生成樹(MST)](https://zh.wikipedia.org/zh-tw/%E6%9C%80%E5%B0%8F%E7%94%9F%E6%88%90%E6%A0%91)

![](/演算法教學/algorithm-Prim/MST.PNG)

首先，類似於Dijkstra的作法，先從圖中挑一個點，把邊都丟進priority_queue中，找出目前的最小邊。

因為我們保證每個點都會被用到所以要連接到一個點，可以從其他任意點過來，也就是可以挑這個點的任意邊。那自然我們會挑最小的那條邊。

接著我們看這個邊連到誰，把邊丟進priority_queue裡面，繼續挑最小的往下做。如果遇到挑最小邊可能成環的狀況，就不要挑這條邊。

因為挑了意味著必須把另一條相對的邊去掉，但是其他會成環的邊，一定是在最小生成樹內，也就是priority_queue篩選出來比這個邊更小的邊，才會形成環，所以這個替換必然會造成權值更大，所以不挑。

ex: 
![](/演算法教學/algorithm-Prim/Prim.PNG)
當我們在挑下一條邊時，最小邊是3，但是他會連回到A點形成環。如果要把這個3的最小邊接起來，勢必要去掉1或2這兩條邊，那這個替換顯然不好，所以我們改挑下一條長度4的邊

## 示範code:
(以上圖作為範例輸入輸出)
時間複雜度$O(NlogN)$
```c++

#include<bits/stdc++.h>
#define pii pair<int,int>
#define F first
#define S second
using namespace std;

const int MAXN = 10;
int n,m;
vector<pii> vec[MAXN];
int vis[MAXN];

int Prim(){

	int ans = 0;
	priority_queue<pii, vector<pii>, greater<pii>> pq; // 用來排列目前最小邊
	pq.push(0,1) // 放入一個點當源點

	while(!pq.empty()){

		pii u = pq.top();
		pq.pop();
		if(vis[u.S]) continue; //這個點已經被挑過
		vis[u.S] = 1;
		ans += u.F // 加入權值

		for(pii i:vec[u]){
			int point = i.F, w = i.S;
			pq.push({w,point}) // 權值放前面，來能用來排序
		}

	return ans;

	}

}

signed main(){

cin>>n>>m;
for(int i=1;i<=m;i++){
	int a,b,w;
	cin>>a>>b,w;
	vec[a].push_back({b,w});
	vec[b].push_back({a,w}); 
}

cout<<Prim()<<endl;

}

```
```bash
input:
5 6
1 3 5
1 4 1
2 4 7122
2 5 2
3 5 5
4 5 5
```
```bash
output:
13
```

---