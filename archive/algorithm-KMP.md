---
title: <演算法筆記> KMP字串演算法
top_img: transparent
date: 2023-09-20 21:40:29
tags:
    - 字串
    - Next表
    - KMP
categories:
    - 演算法筆記
cover: img/cover/algorithm.jpg
description: 講解關於KMP字串演算法的概念與實作
---

## KMP演算法

KMP(Knuth–Morris–Pratt)是一個字串演算法，他可以比對一個小字串、一個大字串，看大字串中是否有出現小字串，出現了幾個，位置在哪裡

ex: 找出abc在abcdabc中，出現了2次，位置分別在s[2]結尾跟s[6]結尾的地方。

## KMP的前導，Next表


在講KMP之前，我們要先來了解甚麼是Next表，Next表是一個陣列，用來儲存一個字串的前綴跟後綴，一樣的東西最長有多長。

先講前綴後綴是甚麼，以abcdefg來講，從a開頭往後延伸的，a, ab, abc...都算是前綴，以g結尾，往前延伸的，g, fg, efg...都算是後綴。

Next表存的就是，到第i個字為止，在他的前後綴的同樣配對中(不包括整個字串做為前綴or後綴)，最長的有多長，以abcdabc舉例:

```bash
abcab
00012 // next表
i=1 no
i=2 no
i=3 no
i=4 'a'bc'a'有長度為1
i=5 'ab'c'ab'有長度為2
當然，也可以有前後綴重疊的時候
ababa在i=5的時候，就有'aba'ba跟ab'aba'，長度為3
```

至於要怎麼找，讓我們從最暴力的方式慢慢優化它。

### 暴力解:

直接一一比對，每個i都做一次，然後從最長的可能狀況枚舉next的長度，如果不成立就-1，一直枚舉到成立或是0，就是最長的的狀況了

```cpp
int next[100];
next[0] = 0; // 第一個數一定會是0
int len = str.length();
for (int i = 1; i < n; i++) // 從第2個數開始
for (int j = i; j >= 0; j--){ // 枚舉長度，總長為i+1，所以最長是i
// substr用來找子字串，參數(開始的位置，子字串長度)
  if (str.substr(0, j) == str.substr(i - j + 1, j)) { // 比對是否一樣
    next[i] = j;
    break;
  }    
}
```

複雜度為$O(N^3)$，枚舉子字串(i)長度$O(N)$，枚舉next長度$O(N)$，比對字串複雜度$O(N)$

## 優化1

在枚舉子字串的過程中，每次都只會比上一次多了一個字元，next最多就是+1，因此我們在枚舉長度的時候，不用每次都從最長枚舉，只要從上一個子字串(i-1)的next值+1做為最長的可能長度就行了。

```cpp
int next[100];
next[0] = 0; 
int len = str.length();
for (int i = 1; i < n; i++) 
for (int j = next[i-1]+1; j >= 0; j--){ // 最長只要枚舉上一個子字串next值+1
  if (str.substr(0, j) == str.substr(i - j + 1, j)) { // 
    next[i] = j;
    break;
  }    
}
```

複雜度為$O(N^2)$，枚舉子字串(i)長度$O(N)$，比對時最糟情況是慢慢累加x次之後，一次跳到0，該次就得讓j從x跑到0，總體來講，會有$O(N/2)的複雜度$

## 優化2(KMP用到的最終優化)

建立在第一個優化之上，我們可以發現，當多加上一個字元時，next不是+1(str[i]!=str[ next[i-1] ])，匹配沒有成功，其實我們不需要用-1,-1,-1...的方式縮減next的長度。

以axacaxax為例，當i=7，準備加入a時，這時前一個的next是3('axa'c'axa')，所以最長next設為4，發現c跟x不匹配。這時候我們會想試試看next=3，不過這是沒有必要的。

因為i=6時的前綴跟後綴長的一樣，都是'axa'，所以不可能選擇next為3的，因為這代表要從後綴取後面2個'xa'(第3個是要加入的a)，從前綴取前面2個'ax'，光是在這裡就不匹配了。

可以看出要找到下一個可能可以用的長度有個關鍵，因為前綴跟後綴的字串一樣，是前一個next取得的子字串，所以可以視為一個單一字串str2。"str2中，必須有前綴跟後綴相同的情況，才有可能去嘗試匹配"

以這題來說，'axa'相同的前後綴是'a'x'a'，所以帶有新的a的next，只有可能前面a或是更短，不會是ax。而i=7的next值也的確是2('ax'acax'ax')

總結來講，當我們要做縮減時，我們應該找到那個上一個子字串中，next字串的next字串，並嘗試，失敗就繼續找，直到值為0

```cpp
int next[100];
next[0] = 0;
string str;

int len = str.length();
for(int i=1;i<len;i++){ 
    int prenext = next[i-1]; // 前一位的最長next
    if(str[i]==str[prenext]){ 
        next[i] = prenext+1;
    }
    else{
        while(str[i]!=str[prenext]){ // 如果不是，則開始找next字串的next長度
            prenext = next[prenext-1]; // next字串的右端點到prenext-1。帶進next陣列的索引就可以得到這個next字串的next值
            if(prenext==0) break;
        }
        if(prenext==0){ // 找不到的情況有兩種，字串尾端可以跟頭匹配，或是不行
            if(str[i]==str[0]) next[i] = 1;
            else next[i] = 0;
        } 
        else{ // 匹配成功的話，值會是該長度+1(長度為新next字串的前面長度，再加上新加入的字元)
            next[i] = prenext+1;
        }
    }
}

```

複雜度是$O(N)$，枚舉子字串(i)長度$O(N)$，找next的複雜度$O(2)$(我不會證明)

## 用Next表來做KMP

知道Next表之後，我們只要把小字串s1跟大字串s2拼在一起，變成s1+'#'+s2就行了。中間的'#'是用來分隔兩個字串的。

透過這個方式，只要在Next表中，看到數值=小字串長度l，就代表這個地方出現了小字串(前綴的l個就是小字串)，並且這個點是出現的小字串結尾。

由此便可以獲悉: 是否出現、數量、位置

模板題: [P3375 【模板】KMP 字符串匹配](https://www.luogu.com.cn/problem/P3375)

```cpp

#include<bits/stdc++.h>
using namespace std;

const int MAXN = 2e6+5;
int nexttable[MAXN];

void next_build(string str){ // 建立next表

    nexttable[0] = 0;
    int len = str.length();

    for(int i=1;i<len;i++){
        int prenext = nexttable[i-1];
        while(str[i]!=str[prenext]){
            prenext = nexttable[prenext-1];
            if(prenext==0) break;
        }
        nexttable[i] = prenext;
        if(str[i]==str[prenext])
            nexttable[i]++;

    }

}

signed main(){

string str1,str2;
cin>>str1;
cin>>str2;

string str = str2 + "#" + str1; // 把兩個字串結合起來，就可以套用next表了

next_build(str); 

for(int i=str2.length()+1;i<str.length();i++){
    if(nexttable[i]==str2.length()) cout<< i - 2*str2.length() + 1<<endl; \
    // 當next表做完之後，找有沒有包含前綴長度str2.length的，有就代表這個字串完整的出現在了以i為右界的後綴(str1)
}
for(int i=0;i<str2.length();i++)
    cout<<nexttable[i]<<" "; // 就照題目輸出str2的next表

}


```

參考資料:[OI Wiki](https://oi-wiki.org/string/kmp/)

---
