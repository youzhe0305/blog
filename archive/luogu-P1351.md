---
title: <洛谷> P1351 [NOIP2014 提高組] 聯合權值
top_img: transparent
date: 2023-05-25 16:36:52
tags:
    - 圖論
    - DFS
    - 普及+/提高
categories:
    - 洛谷
cover: /洛谷/luogu-P1351/104327420.jpg
description: <洛谷> P1351 [NOIP2014 提高組] 聯合權值
---

## 題目: [P1351 [NOIP2014 提高組] 聯合權值](https://www.luogu.com.cn/problem/P3387)

原本看這題的tag，以為會是難題，結果其實滿水的

首先，n個點n-1個邊，顯然是一棵樹，接著他要求距離為2的點對，那顯然是中間跨過一個點，所以我們只要找出每個點作為中間點的情況，然後把他們兩兩組合就好了。

另外，最大值只要把中間點連到的點，找出最大的兩個點相乘，就可以得到這個中間點能創造的最大權值，把每個點比一遍就好。

要注意的是，他的最大值不要求取模，我就被這卡了好一陣子。

```c++

#include <bits/stdc++.h>
using namespace std;

const int MAXN = 2e5 + 5;
const int mod = 10007;
vector<int> tree[MAXN];
int w[MAXN];
int child[MAXN];
int vis[MAXN];
int mx = 0;
int ans = 0;

int DFS(int v) {
  vis[v] = 1;
  int mx_1 = 0;
  int mx_2 = 0;
  for (int i : tree[v]) {

    if (w[i] > mx_1)
      mx_2 = mx_1, mx_1 = w[i];
    else if (w[i] > mx_2)
      mx_2 = w[i];
      
    ans = (ans + (child[v] * w[i]) % mod) % mod;
    child[v] = (child[v] + w[i]) % mod;
    if (!vis[i]) {
      DFS(i);
    }
  }
  mx = max(mx, mx_1 * mx_2);
}

signed main() {

  int n;
  cin >> n;
  for (int i = 1; i <= n - 1; i++) {
    int a, b;
    cin >> a >> b;
    tree[a].push_back(b);
    tree[b].push_back(a);
  }

  for (int i = 1; i <= n; i++)
    cin >> w[i];

  DFS(1);

  cout << mx << " " << (ans * 2) % mod << endl;
}

```

封面圖來源: [虹夏ちゃん](https://www.pixiv.net/artworks/104327420)

---