---
title: <演算法筆記> Kruskal最小生成樹演算法
top_img: transparent
date: 2023-05-29 12:11:10
tags:
    - 圖論
    - 最小生成樹
    - Kruskal
categories:
    - 演算法筆記
cover: img/cover/algorithm.jpg
description: 講解關於Kruskal最小生成樹演算法的概念與實作
---

Kruskal，克魯斯克爾演算法，用貪心的方式，找出一張圖裡面，能連到所有邊的樹中，權值最小的，也就是[最小生成樹(MST)](https://zh.wikipedia.org/zh-tw/%E6%9C%80%E5%B0%8F%E7%94%9F%E6%88%90%E6%A0%91)

![](/演算法教學/algorithm-Kruskal/MST.PNG)

首先，將所有邊排序一遍，依次把最小的邊與其連到的點加入最小生成樹的子圖中，等到所有點都被加入後，就能得到最小生成樹。要注意的是，在加入邊時，要避免子圖中出現環，不然就不會是樹了。

實作上，我們會利用並查集來檢查點是否已經在最小生成樹裡面，以及是否成環。

## 示範code:
(以上圖作為範例輸入輸出)
時間複雜度$O(MlogM)$
```c++

#include<bits/stdc++.h>
#define pip pair<int,pair<int,int>>
#define F first
#define S second
using namespace std;

const int MAXN = 10;
int n,m;
vector<pip> vec; // 儲存邊
int p[MAXN]; // 父節點
int s[MAXN]; // 集合大小

void init(){
	for(int i=1;i<=n;i++)
		p[i] = i, s[i] = 1;
}

int find(int v){
	return p[v] == v ? v : p[v] = find(p[v]);
}

void uni(int a, int b){
	int pa = find(a), pb = find(b);
	p[pa] = pb;
}

int kruskal(){

	int ans = 0;
	sort(vec.begin(),vec.end());
	for(pip i:vec){
		int a = i.S.F, b = i.S.S;
		if(find(a)==find(b)) continue;// 在同個集合下，也就是成環，試著加入重複點
		uni(a,b);
		ans += i.F; // 確定加入最小生成樹後，把權重加入答案
	// 當所有邊都跑完，就可以確定所有點都都被加進去
	}

	return ans;

}

signed main(){

cin>>n>>m;
init();
for(int i=1;i<=m;i++){
	int a,b,w;
	cin>>a>>b>>w;
	vec.push_back({ w, make_pair(a, b) }); // 加入邊(權重放第一位，才能排序)
}

cout<<kruskal()<<endl;

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