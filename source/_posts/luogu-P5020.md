---
title: <洛谷> P5020 [NOIP2018 提高組] 貨幣系統
top_img: transparent
date: 2023-05-23 15:34:54
tags:
    - DP
    - 背包
    - 無限背包
    - 普及+/提高
categories:
    - 洛谷
cover: /洛谷/luogu-P5020/106379296.jpg
description: <洛谷> P5020 [NOIP2018 提高組] 貨幣系統
---

## 題目: [P5020 [NOIP2018 提高組] 貨幣系統](https://www.luogu.com.cn/problem/P5020)

看到這種找錢問題，就是個很標準的無限背包問題，不知道的可以參考[演算法筆記 Coin Change Problem](https://web.ntnu.edu.tw/~algo/KnapsackProblem.html#8)。

不過，從題目的條件中，我們找不到到底該枚舉到多少，去確認他是否可以被湊出來。

讓我們先回到題目來看，他希望我們在保證貨幣系統一樣的情況下，讓貨幣的種類越少越好。這時我們有兩種想法:
1. 構造一個跟原本數組完全無關的數組，不過兩者系統等價
2. 把原本數組簡化，讓他的貨幣種類變少

關於第一種方法，我寫的當下不知道怎麼證明，所以就直接嘗試了第二種方法。關於第一種方法的證明，可以參考[题解 P5020 【货币系统】](https://www.luogu.com.cn/blog/0x3meow/solution-p5020)

至於第二種方法，要把數組簡化，就是把那先不必要出現的貨幣種類刪除，也就是可以被其他貨幣湊出來的。

既然知道這個，那我們就可以知道套回找錢問題中，我們希望判斷是否能被湊出來的最大值，就會是現有貨幣的最大值。此題的a<25000也可以讓我們在無限背包的時間複雜度下完成。

實作上，因為一定是用小面額去湊大面額，所以我們要先排序一遍，從最小的面額開始去湊，過程中，如果能把大面額湊出來，就把他刪掉，最後看剩下多少就是答案了。

```c++

#include <bits/stdc++.h>
using namespace std;

const int MAXN = 25005;

signed main(){

int T;
cin>>T;
while(T--){
	int dp[MAXN]; // 是否能被湊出
	int exist[MAXN]; // 這個面額是否存在
	memset(dp,0,sizeof(dp));
	memset(exist,0,sizeof(exist));
	vector<int> vec;
	int n;
	cin>>n;
	for(int i=1;i<=n;i++){
		int a;
		cin>>a;
		exist[a] = 1;
		vec.push_back(a);
	}
	sort(vec.begin(),vec.end()); // 為了從小開始，排序

	dp[0] = 1;
	for(int i:vec)
	for(int w=i;w<=(*vec.rbegin());w++){ // 最大值為排序完的最後一項
		dp[w] = dp[w] | dp[w-i];
        // 如果可以被別的面額湊出，並且存在，則刪除
		if(dp[w]==1 && exist[w] == 1 && w!=i){
			auto iter = lower_bound(vec.begin(),vec.end(),w);
			vec.erase(iter);
			exist[w] = 0;
		}
	}
	cout<<vec.size()<<endl;
}

}

```

封面圖來源: [BoNiji(ぼ虹)](https://www.pixiv.net/artworks/106379296)

---