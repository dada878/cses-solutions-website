---
sidebar_position: 18
---
Counting Tilings
===

題目
---
有一個$n\times m$的棋盤

你的任務是求出有幾種方式可以用 $1\times 2$ 或 $2\times 1$ 的骨牌鋪滿它

### 輸入
輸入兩個正整數 $n$ 與 $m$，分別代表棋盤的長寬。（$1 \le n \le 10$、$1 \le m \le 1000$）

### 輸出
輸出一個正整數，為答案模 $10^9+7$ 的值

範例測資
---
```
Input : 
4 7

Output :
781
```


想法
---
+ DP
+ 定義 $dp_{i,mask}$ 為第 $1$ 排到第 $i-1$ 排全滿，第 $i$ 排已填完 $mask$ 的方法數

對於從第 $i-1$ 排轉移到第 $i$ 排
1. 先處理 $1\times 2$ 的，因為完成這一步後不能在前一行留空，所以轉移式為
   $dp_{i,mask} = dp_{i-1,\sim mask}$ ("~"表位元反轉)
2. 處理 $2\times 1$ 的
   對於一個狀態，若加入一個 $domino$
   可以從同一排轉移，即
   $dp_{i,mask} =dp_{i,mask}+ dp_{i,mask-domino}$
   要注意為避免不同擺放順序被計為不同，要先枚舉"骨牌的位置"
   如果先枚舉"$mask$"，即範例中交換 `for k` 和 `for j`，會WA

P.S.範例用了滾動DP，但這題不用滾動DP也是可以的

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include<bits/stdc++.h>
#define int long long
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
using namespace std;
const int mod = 1e9+7;

signed main(){
    IO;
    int n,m;
    cin >> n >> m;
    vector<int> dp(1 << n, 0);
    int mask = (1 << n) - 1;
    dp[mask] = 1;
    for(int i = 1; i <= m; i++) {
        for(int j = 0; j * 2 < mask; j++) {
            swap(dp[j] ,dp[mask ^ j]);
        }
        for(int k = 0; k < n - 1; k++) {
            int w = 3 << k;
            for(int j = 0; j <= mask; j++) {
                if((j & w) == w) {
                    dp[j] = (dp[j] + dp[j ^ w]) % mod;
                }
            }
        }
    }
    cout << dp[mask] << "\n";
}
```
</details>