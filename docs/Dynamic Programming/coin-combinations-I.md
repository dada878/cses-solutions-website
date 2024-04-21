---
sidebar_position: 3
---
Coin Combinations I
===

題目
---
有 $n$ 種幣值，有幾種方法可以湊出 $x$ 元 ?

### 輸入
第一行有兩個整數 $n$ 和 $x$，分別代表幣值的數量以及目標要湊出多少錢。

第二行有 $n$ 個相異正整數 $c_i$，代表每種幣值。

- （$1 \le n \le 100$）
- （$1 \le x \le 10^6$）
- （$1 \le c_i \le 10^6$）

### 輸出
求有幾種方法可以湊出 $x$ 元，並輸出答案模 $10^9+7$ 的值

範例測資
---
```
Input:
3 9
2 3 5

Output:
8
```
- $2 + 2 + 5$
- $2 + 5 + 2$
- $5 + 2 + 2$
- $3 + 3 + 3$
- $2 + 2 + 2 + 3$
- $2 + 2 + 3 + 2$
- $2 + 3 + 2 + 2$
- $3 + 2 + 2 + 2$

總共 $8$ 種

想法
---
舉個例子，假設有其中一種幣值為 $5$，且要湊出 $12$ 元，那麼我們可以得知湊出 $12$ 元的方法是湊出 $12$ 的方法數加上湊出 $7$ 元的方法數

設 $dp_j$ 的狀態為湊出 $j$ 元所需要的最少硬幣數量，接著枚舉每一種幣值，若能夠湊出 $j - c_i$ 元，那麼可以推得轉移式為 $dp_j += dp_{j - c_i}$

**Note 1:** 注意 $j - c_i$ 是否小於 $0$

**Note 2:** 初始狀態為 $dp_0 = 0$，因為湊出總合為 $0$ 需要 $0$ 個硬幣 

### 範例程式碼
<details>
<summary>C++ 範例 </summary>
```cpp
#include<bits/stdc++.h>
#define int long long
#define IO ios_base::sync_with_stdio(0),cin.tie(0)
const int MOD = 1e9+7;
using namespace std;

signed main() {
    IO;
    int n, x;
    cin >> n >> x;
    vector<int>c(n);
    vector<int>dp(x + 1, 0);
    for(int i = 0; i < n; i++) {
        cin >> c[i];
    }
    dp[0] = 1;
    for(int i = 1; i <= x; i++) {
        for(int j = 0; j < n; j++) {
            if(i - c[j] >= 0) {
                dp[i] += dp[i - c[j]];
                dp[i] %= MOD;
            }
        }
    }
    cout << dp[x];
}
```
</details>