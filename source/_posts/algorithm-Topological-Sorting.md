---
title: <演算法教學> 拓樸排序演算法
top_img: transparent
date: 2023-05-29 10:00:00
tags:
    - 圖論
    - DAG
categories:
    - 演算法教學
cover: 演算法教學/algorithm-Topological-Sorting/108530112.jpg
description: 講解關於拓樸排序演算法的概念與實作
---

拓樸排序(Topological Sorting)用於[有向無環圖(DAG)](https://zh.wikipedia.org/zh-tw/%E6%9C%89%E5%90%91%E6%97%A0%E7%8E%AF%E5%9B%BE)上，可以排出一個節點順序，保證每個節點被經過時，前面指向他的節點都已經被經過。

可以用於任務有前置條件時，對任務的排序，或是最長路徑的計算。

示例圖:
(黑字:點編號, 紅字:執行順序)

![](/演算法教學/algorithm-Topological-Sorting/task_sort.PNG)


拓樸排序的核心在於，不斷找出入度為0的點，也就是沒有任何前置任務，可以直接執行的任務。

一開始，我們先找到初始入度為0的點，並把他加入我們的工作執行序列裡。接著，當我們從工作執行序列裡，將其取出並執行完成後，把它指向的節點，入度全部減一，代表前置任務被完成了一項。如果這些節點有入度成為0的點，代表他也可以直接執行，所以加入工作執行序列中，就這樣一路做到底，就完成了拓樸排序了。

示範code:
(以上圖為例)
```c++

#include<bits/stdc++.h>
using namespace std;

const int MAXN = 10;
int n,m;
int rd[MAXN]; // 入度
vector<int> vec[MAXN]; // 存圖

void do_func(int x){ // 執行函式
	cout<<x<<endl;
}

void topo(){

	queue<int> que; // 工作序列

	for(int i=1;i<=n;i++)
		if(rd[i]==0) que.push(i);

	while(!que.empty()){
		int u = que.front();
		que.pop();
		do_func(u); // 執行這個任務
		for(int i:vec[u]){
			rd[i]--; // 因為前一個任務被執行完，所以入度減一
			if(rd[i]==0) que.push(i); // 沒有前置序列，加入最長路
		}
	}

}

signed main(){

	cin>>n>>m; // 點數,邊數
	for(int i=1;i<=m;i++){
		int a,b;
		cin>>a>>b;
		vec[a].push_back(b);
		rd[b]++; // 被指向，所以b的入度+1
	}

	topo();

}


```

```bash
input:
5 5
1 2
1 3
2 4
3 5
4 5
```

```bash
output:
1
2
3
4
5
```

喔對，還有今天是虹夏的生日，都來祝他生日快樂

![](/演算法教學/algorithm-Topological-Sorting/108530112.jpg)

[虹夏ちゃん誕生日おめでとう🎉🎉🎉【2023】](https://www.pixiv.net/artworks/108530112)

---