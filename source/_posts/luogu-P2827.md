---
title: <洛谷> P2827 [NOIP2016 提高組] 蚯蚓
top_img: transparent
date: 2023-05-18 11:57:00
tags: 
    - 單調
    - priority_queue
    - 提高+/省選-
categories:
    - 洛谷
cover: /img/106276835.jpg
description: <洛谷> P2827 [NOIP2016 提高組] 蚯蚓
---

題目: [P2827 [NOIP2016 提高組] 蚯蚓](https://www.luogu.com.cn/problem/P2827)

## 90分解:

看到這個題目，我第一個想到的就是用**priority_queue**來維護，一個一個找出最長的蟲子，並做操作。

其中，需要處理的是，每隻蟲子會成長的問題。因為不可能把資結裡面的所有蟲子都加上成長量，因此我們換個思路。因為在資結裡的資料，重點是要比大小，因此只要能讓他的大小順序保持住就好。那我們就不對資料結構裡面的所有蟲加上成長量，反過來把新加入的蟲(切出來的)減去成長量，他們之間的相對值就能維持住。另外開一個變數來記錄總成長量，當我們需要求蟲的長度時，再把成長量加回去就可以得到值了。

這個方法的複雜度是$O(MlogN)$，我本來預期會過，不過出題者好像把這個複雜度卡掉了。如果壓常很強的話，搞不好可以透過這個方法AC?

```c++

#include<bits/stdc++.h>
using namespace std;

priority_queue<int,vector<int>,less<int>> pq;
int n,m,q,t;
double u,v;
double p;

signed main(){
	ios_base::sync_with_stdio(0);
	cin.tie(0);
	cin>>n>>m>>q>>u>>v>>t;
	int num = n; // 用來記錄目前有幾隻蟲
	p = u/v;
	for(int i=1;i<=n;i++){
		double temp;
		cin>>temp;
		pq.push(temp); 
	}
	int len_add = 0;
	for(int i=1;i<=m;i++){
		int longest = pq.top() + len_add; 
		pq.pop();
		if(i%t==0){
			cout<<longest<<" ";
		}
		num--;
		len_add += q; // 總成長量為 m*q
		int a = int(longest * p);
		int b = longest - a;
		pq.push(a - len_add ); // 減去總成長量，等於前面這一段時間的成長量都沒有吃掉
		pq.push(b - len_add);
		num += 2;
	}
	cout<<endl;
	for(int i=1;i<=num;i++){
		if(i%t==0)
			cout<<pq.top() + len_add<<" "; // 因為有減去，所以拿出來的時候，加回來
		pq.pop();
	}

}

```

## 100分解: 

這個方法是利用單調性來維護，就可以去掉$logN$的複雜度，把剩下的十分拿下。

不過這是我看題解思路寫的，我自己真的沒有觀察出他有單調性。

首先，要觀察出一件事，**先被切出來的蟲，一定比後被切出來的蟲長**
假設目前最大的兩隻蟲為$a,b$，$a>b$
過一秒後，$a$被切成$pa,(1-p)a$，$b$成長為$b+q$
再過一秒，$a$的兩條蟲成長為$pa+q,(1-p)a+q$，$b$被切成$pb+pq,(1-p)b+(1-p)q$
因為$a>b$，$0 > p > 1$，所以$pa+q > pb+pq$，$(1-p)a+q > (1-p)b+(1-p)q$
可以發現切出來的蟲中，較長蟲一定比後面切出的較長蟲更長，較短蟲一定比後面切出的較短蟲更長
接下來兩者的成長都是同時發生，所以相對大小不變
發現他的單調性，再把它分成另外兩堆，就可以不用priority_queue來維護了
找最長的蟲時，只要從這兩堆，跟原本還沒被切過的那堆(3堆)中，挑出來最大的就行了

```c++

#include<bits/stdc++.h>
using namespace std;

deque<int> non_dq; // 沒被切的蟲
deque<int> dq_min; // 切出來較短的蟲
deque<int> dq_max; // 切出來較長的蟲
int n,m,q,t;
double u,v;
double p;
int len_add = 0;

int find_longest(){ // 從3堆中找最大值得函式，我寫的比較醜一點
	int longest;
	if(dq_max.empty()){
		if(dq_min.empty())
			longest = non_dq.back() + len_add,non_dq.pop_back();
		else if(non_dq.empty())
			longest = dq_min.back() + len_add,dq_min.pop_back();
		else{
			if(non_dq.back() >= dq_min.back())
				longest = non_dq.back() + len_add,non_dq.pop_back();
			else
				longest = dq_min.back() + len_add,dq_min.pop_back();
		}
	}
	else if(dq_min.empty()){
		if(dq_max.empty())
			longest = non_dq.back() + len_add,non_dq.pop_back();
		else if(non_dq.empty())
			longest = dq_max.back() + len_add,dq_max.pop_back();
		else{
			if(non_dq.back() >= dq_max.back())
				longest = non_dq.back() + len_add,non_dq.pop_back();
			else
				longest = dq_max.back() + len_add,dq_max.pop_back();
		}
	}
	else if(non_dq.empty()){
		if(dq_max.empty())
			longest = dq_min.back() + len_add,dq_min.pop_back();
		else if(dq_min.empty())
			longest = dq_max.back() + len_add,dq_max.pop_back();
		else{
			if(dq_max.back() >= dq_min.back())
				longest = dq_max.back() + len_add,dq_max.pop_back();
			else
				longest = dq_min.back() + len_add,dq_min.pop_back();
		}
	}
	else{
		if(dq_min.back() >= dq_max.back() && dq_min.back() >= non_dq.back())
			longest = dq_min.back() + len_add,dq_min.pop_back();
		else if(dq_max.back() >= dq_min.back() && dq_max.back() >= non_dq.back())
			longest = dq_max.back() + len_add,dq_max.pop_back();
		else
			longest = non_dq.back() + len_add,non_dq.pop_back();
	}
	return longest;
}

signed main(){
	ios_base::sync_with_stdio(0);
	cin.tie(0);
	cin>>n>>m>>q>>u>>v>>t;
	int num = n;
	p = u/v;
	for(int i=1;i<=n;i++){
		double temp;
		cin>>temp;
		non_dq.push_back(temp);
	}
	sort(non_dq.begin(), non_dq.end());
	for(int i=1;i<=m;i++){
		int longest = find_longest();
		if(i%t==0){
			cout<<longest<<" ";
		}
		num--;
		len_add += q;
		int a = int(longest * p);
		int b = longest - a;
		if(a<b) swap(a,b); // 保持a是比較長的那隻
		dq_max.push_front(a - len_add); // 把較長的蟲，跟較短的蟲分開維護
		dq_min.push_front(b - len_add);
		num += 2;
	}
	cout<<endl;
	for(int i=1;i<=num;i++){
		int longest = find_longest();
		if(i%t==0)
			cout<<longest<<" ";
	}

}

```

封面圖來源: [虹夏ちゃん✨](https://www.pixiv.net/artworks/106276835)

---
