---
sidebar_position: 7
---

Book Shop
===

題目
---
你在一間賣 $n$ 本書的書店裡，你知道每本書的價格以及頁數，並且你有 $x$ 元的預算，請問你最多能買到多少頁的書 ? （每本書只能買一次）

### 輸入
- 第一行輸入兩個正整數 $n$ 和 $x$，分別代表總共有幾本書，以及你的預算。（$1 \le n \le 1000$ , $1 \le x \le 10^5$）
- 第二行有 $n$ 個正整數 $h_i$，代表第 $i$ 本書的價格。（$1 \le h_i \le 1000$）
- 第三行有 $n$ 個正整數 $s_i$，代表第 $i$ 本書的頁數。（$1 \le s_i \le 1000$）

### 輸出
輸出一個正整數，為最多能買到的頁數。

範例測資
---
```
Input:
4 10
4 8 5 3
5 12 8 1

Output:
13
```

買了第 $1$ 和 $3$ 本書，會花費 $4 + 5 = 9$ 元，且能買到 $5 + 8 = 13$ 頁

想法
---
因為每本書只能買一次，所以要用好的轉移順序才能避免掉重複選取的問題。

假設今天要花 $3$ 元，並且考慮能不能買一本 $1$ 元 / 1 頁的書，從 $1$ 元開始轉移的話會變成 : 

- $dp[1] = dp[1] + dp[0] = 1$
- $dp[2] = dp[2] + dp[1] = 1$
- $dp[3] = dp[3] + dp[2] = 1$

但是如果從 $3$ 開始轉移的話會變成 :

- $dp[3] = dp[3] + dp[2] = 0$
- $dp[2] = dp[2] + dp[1] = 0$
- $dp[1] = dp[1] + dp[0] = 1$

由以上的例子可以發現從 $1$ 開始枚舉轉移點的話會有重複計算的問題


### 狀態定義
設 $dp[j]$ 的狀態為花了 $i$ 元能買到的最大頁數

### 狀態轉移
用 $j - h_i$ 能買到的頁數再加上 $s_i$ 即為買了第 $i$ 本書使得當前已花費 $j$ 元能夠得到的最大頁數，轉移式為 : $dp[j] = max(dp[j], dp[j - h_i] + s_i)$

### 範例程式碼
<details>
<summary>C++ 範例 </summary>

```cpp
#include <bits/stdc++.h>
#define int long long 
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
using namespace std;

int h[1005], s[1005], dp[100005];

signed main() {
    IO;
    int n, x;
    cin >> n >> x;
    for(int i = 0; i < n; i++) {
        cin >> h[i];
    }
    for(int i = 0; i < n; i++) {
        cin >> s[i];
    }
    for(int i = 0; i < n; i++) {
        for(int j = x; j >= h[i]; j--) {
            dp[j] = max(dp[j], dp[j - h[i]] + s[i]);
        }
    }
    cout << dp[x];
}
```
</details>