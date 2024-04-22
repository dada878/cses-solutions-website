---
sidebar_position: 5
---
Removing Digits
===

題目
---
給定一正整數 $n$，且每次操作都能將一個一個數字減去他自己的其中一個位數的數字，求最少需要幾次操作能讓該數字變成 $0$ ?

### 輸入
輸入一個正整數 $n$。（$1 \le n \le 10^6$）

### 輸出
輸出最少需要經過幾次操作使 $n$ 變成 $0$

範例測資
---
```
Input:
27

Output:
5
```
- $27 - 7 = 20$
- $20 - 2 = 18$
- $18 - 8 = 10$
- $10 - 1 = 9$
- $9 - 9 = 0$

總共經過 $5$ 次操作

想法 1 : 貪心
---

相信看完範例測資後大部分的人都會直覺想到貪心解法，也就是每次都減掉所有位數裡面最大的那一個即可。

> 證明 : 假設 $0 \sim k$ 答案非遞減，
$k + 1$ 的最大位元最多比 $k$ 的最大位元大 $1$，
所以 $k + 1$ 沒法轉移到比k可以轉移到的數小的數，
因此 $k + 1$ 的答案不會比 $k$ 小，
數學歸納法可證。

### 範例程式碼

<details>
<summary>C++ 範例 </summary>

```cpp
#include <bits/stdc++.h>
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
#define int long long 
using namespace std;

signed main() {
    IO;
    int n, ans = 0;
    cin >> n;
    while(n > 0) {
        int big = -1;
        for(int i = n; i > 0; i /= 10) {
            big = max(big, i % 10);
        }
        n -= big;
        ans++;
    }
    cout << ans;
}
```
</details>

想法 2 : DP
---

從測資我們可以知道 $27$ 可以從兩個地方轉移過來，分別是 $20$ 以及 $25$，因此只要先求出 $20$ 和 $25$ 所需要的最小操作次數就可以推得 $27$ 所需要的最小操作次數。

### 作法
從 $i$ 等於 $1$ 開始計算每個數字所需要的最小操作次數，就可以確保之後的數字一定有轉移點

### 狀態定義
設 $dp[i]$ 為將 $i$ 變成 $0$ 所需要的最小操作次數

### 狀態轉移
對於每個位數的數字 $C_i$，轉移式為 $dp[i] = min(dp[i], dp[i - C_i])$

### 範例程式碼

<details>
<summary>C++ 範例 </summary>

```cpp
#include <bits/stdc++.h>
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
#define int long long 
using namespace std;
 
signed main() {
    IO;
    int n;
    cin >> n;
    vector<int>dp(n + 1, INT_MAX);
    dp[0] = 0;
    for(int i = 1; i <= n; i++) {
        for(int j = i; j > 0; j /= 10) {
            dp[i] = min(dp[i], dp[i - (j % 10)] + 1);
        }
    }
    cout << dp[n];
}
```
</details>