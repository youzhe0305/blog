---
title: <演算法筆記> 並查集資料結構
top_img: transparent
date: 2023-05-29 11:27:38
tags:
    - 圖論
    - 集合
    - 並查集
categories:
    - 演算法筆記
cover: img/cover/algorithm.jpg
description: 講解關於並查集資料結構的概念與實作
---

並查集(Disjoint-set)，用來處理物品是否同一群的資料結構，他可以做到合併、查詢集合的功能，所以稱為並查集。

我們透過紀錄每個點的父節點來達成，當兩個點被歸類到同一集合時，我們以這個集合的根最為父節點，我這裡稱為最終父節點。

查詢時，如果有兩個點的最終父節點相同，即代表他們在同個集合裡。

合併時，把其中一個集合的最終父節點，定為另一個集合的最終父節點，讓兩者最終父節點相同，即達成合併。

另外，合併有個優化的方法，稱為啟發式合併，在啟發式合併裡，我們選擇把比較小的集合併到比較大的集合中，這樣當這個比較小的集合要更新成新的父節點時，只要走更少的路。

這東西直接看code會更好懂一點。

## 示範code
```c++

int n,m;
int p[MAXN]; // 節點的父節點
int s[MAXN]; // 集合大小(size)

void init(){ //　初始化先把自己的父節點設為自己，並把集合大小設為1
	for(int i=1;i<=n;i++)
		p[i] = i, s[i] = 1;
}

int find(int v){ // 查詢父節點
	// 看父節點是不是自己，不是就往上找父節點，直到最終父節點(父節點=自己)
	return p[v] == v ? v : p[v] = find(p[v]); // 順便把點的父節點更新成最終父節點
}

void uni(int a, int b){ // 合併兩個集合
	// 讓a集合的最終父節點，變成b集合的最終父節點
	//  讓a集合並到b集合上
	int pa = find(a), pb = find(b);
	p[pa] = pb;
}

void uni_inspire(int a,int b){ // 啟發式合併版本
	int pa = find(a), pb = find(b);
	if(s[pa]>s[pb]) swap(pa,pb) // 保持b團體更大
	p[pa] = pb; // 把小的a集合並到b集合上
	s[pb] += s[pa]; // 把size相加
	s[pa] = 0;
}


```


---