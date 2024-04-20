---
sidebar_position: 1
---
Dice Combinations
===

題目
---
有幾種方法可以用骰子骰出點數 $n$ ? 

骰子只會骰出 $1$ 到 $6$ 的數字。

### 輸入
輸入一個正整數 $n$ 。（$1 \le n \le 10^6$）

### 輸出
方法數對 $10^9 + 7$ 取餘數

範例測資
---
```
Input:
3

Output:
4
```
- $1 + 1 + 1$
- $1 + 2$
- $2 + 1$
- $3$

共 $4$ 種方法

想法
---
假設要骰出數字 $18$，有以下 $6$ 種方法 : 

- 原本骰出 $12$ 後再骰出 $6$
- 原本骰出 $13$ 後再骰出 $5$
- 原本骰出 $14$ 後再骰出 $4$
- 原本骰出 $15$ 後再骰出 $3$
- 原本骰出 $16$ 後再骰出 $2$
- 原本骰出 $17$ 後再骰出 $1$

設 $dp[i]$ 為湊出點數為 $i$ 的方法數。
藉由上面的例子，我們可以得知 $dp[i] = \displaystyle\sum_{j = i - 6}^{i - 1} dp[j]$ 

**Note 1 :** $dp[0] = 1$，因為只有一種方法可以湊出點數 $0$ (也就是沒有骰骰子)

**Note 2:** $i = 5$ 以前的 $dp[i]$ 都要注意，轉移狀態時不能讓 $j < 0$

### 範例程式碼
<details>
<summary>C++ 範例 </summary>
```cpp
#include <bits/stdc++.h>
#define int long long 
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
using namespace std;
const int mod = 1e9 + 7, sz = 1e6 + 5;
int n, dp[sz];

signed main() {
    IO;
    cin >> n;
    dp[0] = 1;
    for(int i = 1; i <= n; i++) {
        for(int j = 1; j <= 6; j++) {
            if(i - j >= 0) {
                dp[i] += dp[i - j];
                dp[i] %= mod;
            }
        }
    }
    cout << dp[n];
}
```
</details>