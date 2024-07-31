---
sidebar_position: 19
---

Counting Numbers 
===

## 題目敘述
給定兩個正整數 $a,\ b$，求 $[a,\ b]$ 有多少正整數中，任何相鄰的數字皆不相同。

> 輸入、輸出的說明省略

## 觀察
1. 數值範圍非常大
2. 尋找一個區間內，有某性質的整數數量（計數問題）

以上兩個就是「[數位 DP](https://oi-wiki.org/dp/number/)」的特徵。

## 解法
### DP 狀態
數位 DP 通常會有一個模板化的狀態設計：$dp_{pos,limit,lead,*}$

- $limit$: 布林值，是否到達「上界」
- $pos$: 整數，目前處理到後 $pos$ 位的答案
	- 假設 $n = 12345$
	- 若 $pos = 2$ 且 $limit = true$，則該為計算 $0000 \sim 2345$ 的答案
	- 若 $pos = 2$ 且 $limit = false$，則該為計算 $0000 \sim 9999$ 的答案
- $lead$: 目前的位數是否在前綴零裡面
	- 前綴零在大多數題目都是可以不用符合性質，因此特別判斷

本題還需引入一個狀態：
- $pre$: 當前一位是 $pre$ 時，答案的數量（設第一位的前一位是 $-1$）

### DP 轉移
同樣的，數位 DP 通常用記憶化搜索的方式轉移。

請注意下方程式碼第 29 行 `memorize_search(s, pos+1, now, limit&(now==(s[pos]-'0')), lead&(now==0));`

- `limit&(now==(s[pos]-'0'))`：前面的位數要是 $limit = true$，且該位數也達到上界才是 $true$
- `lead&(now==0)`：前面的位數要是在前綴零裡面，且該位數也是 $0$ 才是 $true$

![image](https://hackmd.io/_uploads/rySLKPTQ0.png)

### 範例程式碼

<details>
<summary>C++ 範例程式碼</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

long long l, r;
long long dp[20][10][2][2]; // dp[pos][pre][limit][lead] = 後 pos 位，pos 前一位是 pre，（是/否）有上界，（是/否）有前綴零的答案數量

long long memorize_search(string &s, int pos, int pre, bool limit, bool lead){

    // 已經被找過了，直接回傳值
    if (dp[pos][pre][limit][lead]!=-1) return dp[pos][pre][limit][lead];

    // 已經搜尋完畢，紀錄答案並回傳
    if (pos==(int)s.size()){
        return dp[pos][pre][limit][lead] = 1;
    }

    // 枚舉目前的位數數字是多少
    long long ans = 0;
    for (int now=0 ; now<=(limit ? s[pos]-'0' : 9) ; now++){
        if (now==pre){

            // 1~9 絕對不能連續出現
            if (pre!=0) continue;

            // 如果已經不在前綴零的範圍內，0 不能連續出現
            if (lead==false) continue;
        }

        ans += memorize_search(s, pos+1, now, limit&(now==(s[pos]-'0')), lead&(now==0));
    }

    // 已經搜尋完畢，紀錄答案並回傳
    return dp[pos][pre][limit][lead] = ans;
}

// 回傳 [0, n] 有多少數字符合條件
long long find_answer(long long n){
    memset(dp, -1, sizeof(dp));
    string tmp = to_string(n);

    return memorize_search(tmp, 0, 0, true, true);
}

int main(){

    // input
    cin >> l >> r;

    // output
    cout << find_answer(r)-find_answer(l-1) << "\n";

    return 0;
}
```

</details>