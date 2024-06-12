---
title: <演算法筆記> SPFA單點源最短路演算法
top_img: transparent
date: 2023-05-22 17:05:22
tags:
    - 圖論
    - 最短路
    - SPFA
categories:
    - 演算法筆記
cover: img/cover/algorithm.jpg
description: 講解關於SPFA最短路演算法的概念與實作
---

SPFA(Shortest Path Faster Algorithm)，最短路徑快速算法，帶有queue優化的Bellman-Ford算法。(因為SPFA比較常用，就直接講SPFA，Bellman-Ford就請有興趣的人去再去研究)

這個演算法通常用來解決圖上可能有負環的情況，也就是我們上一篇提到，Dijkstra演算法無法解決的狀況。讓我們先來看看，有負環的情況下，會發生甚麼事?

![](/演算法教學/algorithm-SPFA/negative-loop.gif)

可以發現他會進入一個無限迴圈，一直在負環裡面跑，讓負環上的點的距離值不斷被更新、變小。

也就是說，存在負環的狀況下，我們不可能找到最短路徑，因為路徑會不斷變短。所以我們要使用SPFA來解決這個問題，判斷負環是否存在。

判斷負環是否存在的關鍵，在於更新次數。首先讓我們想想一個點最多被更新幾次?顯然是n-1次，因為只能被其他點各鬆弛一次，當一直被重複更新時，更新次數就會超過n-1，也就是出現了負環，SPFA也就是針對此判斷。

SPFA與Bellman-Ford差別最大的是在於queue的優化，Bellman-Ford會直接不管三七二十一的，直接跑n-1次。這裡則利用類似BFS的方式來做優化，讓他不用跑滿n-1次。

SPFA很像BFS，就是從起點開始，一直往下跑，看下面的點能不能被當前的點更新到，最大的差別是，SPFA是可以往回跑到重複的點的，也就是可以在後面更新出更短的路徑時，跑回去更新前面的點，就因此解決了BFS只能解決邊權唯一圖形的狀況。

跑的過程中，如果可以更新，就記錄更新次數加一，如果更新次數超過了n-1，代表重複更新，有負環存在。如果沒有超過的話，更新完的值也是更新到不能再更新了。

其中，我們在queue的使用上，會用inque來記錄這個點是否在queue裡面，如果一個點已經在queue裡面，代表他即將被用來更新其他點，這時候就算被更新一個新的距離，也不必再把他丟進去一次，只要把distance陣列中的值改掉，在更新的時候就會使用最小的值。

ex: 有a->c,距離10的路，b->c距離5的路，假設dis[a]=3,dis[b]=5，dis[c]先被更新成dis[a]+10=13丟進queue裡面，接著dis[c]又被更新成dis[b]+5=10，這時候就不用再丟進queue一次，因為從queue拿出來的時候，會以當下的dis[c]來鬆弛其他點，也就是dis[c]=10。

## 示範code:
時間複雜度$O(NM)$
```c++

int dis[MAXN]; // 距離
queue<int> que; 
int inque[MAXN]; // 記錄這個點是不是在queue裡面
int num[MAXN];

int SPFA(int v,int n){ // 以v當作起點，有n個點
	dis[v] = 0;
	que.push(v);
	inque[v] = 1; // 點v進入queue
	while(!que.empty()){

		int u = que.front();
		que.pop();
		inque[u] = 0; // 點v離開queue

		for(pii i:vec[u]){
			if(dis[i.F] > dis[u]+i.S){ // i.S 表示邊權
				num[i.F]++;
				if(num[i.F] > n-1){
					return 0; // 如果更新次數大於n-1，代表重複更新，有負環存在
				}
				dis[i.F] = dis[u]+i.S;
				if(inque[i.F]==0){
					que.push(i.F);
					inque[i.F]=1;
				}

			}
		}

	}
	return 1;
}

```

---
