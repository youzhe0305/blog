---
title: <演算法教學> Dijkstra單點源最短路演算法
top_img: transparent
date: 2023-05-22 16:59:41
tags:
    - 圖論
    - 最短路
    - Dijkstra
categories:
    - 演算法教學
cover: 演算法教學/algorithm-Dijkstra/103214931.jpg
description: 講解關於Dijkstra最短路演算法的概念與實作
---

Dijkstra演算法，戴克斯特拉演算法，用來解決單點源最短路問題的演算法。

講Dijkstra之前，我們要先了解鬆弛的概念，鬆弛就是兩個點之間，出現了一條更短路徑。舉例來講，a到b的距離是10，a到c到b的距離是8，因為出現了更短的路徑，因此我們說，c鬆弛了a到b。

Dijkstra的核心在於，我們在找最短路時，每次都選擇目前距離最近的點。因為其他點的距離都更遠，不可能去利用其他點來鬆弛這個點，所以這個點現在的距離保證會是他的最短距離。之後我們可以再利用這個保證是最短路徑的點，去試著鬆弛其他點。

![](/演算法教學/algorithm-Dijkstra/Dijkstra.gif)

實作上，我們會利用priority_queue來達成，我們會把每個新被鬆弛出來的點，丟進去priority_queue裡面，並且把裡面最小的取出來，再用這個點去鬆弛其他點。

順帶一提，Dijkstra不能用在有負環的圖上，關於這個，我們會在下一篇的SPFA演算法中提到

## 示範code:
時間複雜度$O(NlogN)$
```c++

vector< pair<int,int> > gra[MAXN]; // 圖
int dis[MAXN]; // 記錄距離
int vis[MAXN]; // 記錄是不是確定是最短路徑，並且鬆弛過別人了

priority_queue< pair<int,int> ,vector< pair<int,int> >,greater< pair<int,int> > > pq;
// greater 代表由小到大
// 裡面的儲存格式為{dis[a],a}，把點權放前面是因為priority_queue會優先以first項來排序

void dijkstra(int s){
    // 如果已經確定是最短路徑，後面得到的值一定不是最短的，直接丟掉換下一個
    if(vis[s]){ 
        pq.pop();
        if(!pq.empty())
        dijkstra(pq.top().S);
        return;
    }
    vis[s] = 1; // 被拿來鬆弛其他人

    pq.pop();
    for(pair<int,int > i:gra[s]){
        if(dis[i.F]>dis[s]+i.S){ // 鬆弛成功 // i.S表示邊權
            dis[i.F] = dis[s]+i.S; // 更新距離
            pq.push({dis[i.F],i.F}); // 有了更小的距離，因此丟進priority_queue中比較
        }
    }

    if(!pq.empty())
        dijkstra(pq.top().S); // 找下一個目前最近的點是誰
        return;
    }

```

封面圖來源: [虹夏](https://www.pixiv.net/artworks/103214931)

---
