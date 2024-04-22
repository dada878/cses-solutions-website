---
sidebar_position: 2
---
Minimizing Coins
===

題目
---
有 $n$ 種幣值，最少可以用幾個硬幣湊出 $x$ 元 ?

### 輸入
第一行有兩個整數 $n$ 和 $x$，分別代表幣值的數量以及目標要湊出多少錢。

第二行有 $n$ 個相異正整數 $c_i$，代表每種幣值。

- （$1 \le n \le 100$）
- （$1 \le x \le 10^6$）
- （$1 \le c_i \le 10^6$）

### 輸出
最少需要多少個硬幣才能湊出 $x$ 元 ?

若沒有辦法湊出 $x$ 元，輸出 $-1$

範例測資
---
```
Input:
3 11
1 5 7

Output:
3
```
$1 + 5 + 5 = 11$

共 $3$ 個硬幣湊出 $11$ 元

想法
---
舉個例子，假設有其中一種幣值為 $5$，且要湊出 $12$ 元，那麼我們可以得知湊出 $12$ 元的方法是湊出 $7$ 元所需要的硬幣數量再加一（加上剛剛的 $5$ 元），或是原本有更好的方案可以湊出 $12$ 元就保留原本的方案。

### 狀態定義
設 $dp_j$ 的狀態為湊出 $j$ 元所需要的最少硬幣數量

### 狀態轉移
枚舉每一種幣值，若能夠湊出 $j - c_i$ 元，那麼可以推得轉移式為 $dp_j = min(dp_{j - c_i} + 1, dp_j)$ 

**Note 1:** 注意 $j - c_i$ 是否小於 $0$

**Note 2:** 初始狀態為 $dp_0 = 0$，因為湊出總合為 $0$ 需要 $0$ 個硬幣 

### 範例程式碼
<details>
<summary>C++ 範例 </summary>
```cpp
#include<bits/stdc++.h>
#define int long long
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
using namespace std;
signed main() {
    IO;
    int n, x;
    cin >> n >> x;
    int c[n];
    for(int i = 0; i < n; i++) {
        cin >> c[i];
    }
    int dp[x + 1];
    for(int i = 1; i <= x; i++) {
        dp[i] = -1;
    }
    dp[0] = 0;
    for(int i = 1; i <= x; i++) {
        for(int j = 0; j < n; j++) {
            if(i - c[j] >= 0) {
                if(dp[i - c[j]] != -1) {
                    if(dp[i] == -1) {
                        dp[i] = dp[i - c[j]] + 1;
                    }
                    else {
                        dp[i] = min(dp[i], dp[i - c[j]] + 1);
                    }
                }
            }
        }
    }
    cout << dp[x];
}
```
</details>