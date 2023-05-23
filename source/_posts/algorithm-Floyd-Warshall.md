---
title: <演算法教學> Floyd-Warshall多點源最短路演算法
top_img: transparent
date: 2023-05-22 22:46:11
tags:
    - 最短路
    - Floyd-Warshall
categories:
    - 演算法教學
cover: 演算法教學/algorithm-Floyd-Warshall/106493951.jpg
description: 講解關於Floyd-Warshall最短路演算法的概念與實作
---

Floyd-Warshall演算法，弗洛伊德演算法，用來解決多點源最短路問題的演算法。

相較於Dijkstra及SPFA只能找到對於一個起點的最短路徑，Floyd-Warshall可以用來找到圖上任意點對任意點的最短路徑。

Floyd-Warshall的概念相對起來好懂很多，就是幹下去就對了，用每一個點去鬆弛每一個點。直接用for迴圈枚舉就行，就直接看code吧

## 示範code:
時間複雜度$O(N^3)$
```c++

int dp[MAXN]; // 表示最短距離
void Floyd-Warshall(){  
    for(int k=1;k<=n;k++)
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++){
                if(dp[i][j] > dp[i][k] + dp[k][j]){
                    dp[i][j] = dp[i][k] + dp[k][j]; // 當可以被鬆弛，則更新
                }    
}

```

封面圖來源: [ん…ちょっと寂しかった……](https://www.pixiv.net/artworks/106493951)

---