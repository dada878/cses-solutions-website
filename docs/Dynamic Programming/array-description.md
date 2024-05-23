---
sidebar_position: 8
---

Array Description
===

題目
---
你有 $n$ 項的整數數列，每個整數都在 $1$ 到 $m$ 之間，其中任兩個相鄰數字的數值相差最多 $1$。
給你這個數列的其中幾項，其他項未知，你的任務是計算有多少種可能的數列滿足條件。

### 輸入
- 第一行輸入兩個正整數 $n$ 與 $m$，分別代表數列的長度和數列裡每個數字的最大值。（$1 \le n \le 10^5$、$1 \le m \le 100$）
- 第二行會有 $n$ 個整數 $x_1,x_2,\dotsc,x_n$，如果 $x_i=0$ 代表此項是未知的。（$0 \le x_i \le m$）

### 輸出
可能數列的數量模 $10^9 + 7$。

範例測資
---
```
Input : 
3 5
2 0 2

Output :
3
```

總共有 $[2,1,2]$、$[2,2,2]$、$[2,3,2]$ 三種可能的數列，所以答案為 $3$。

想法
---
### 觀察
觀察到 $O(nm)$ 的時間複雜度是好的，可以用 $O(nm)$ 時間複雜度的 dp 來解這一題。

### 想法
利用分而治之的概念，我們先找到數列 $x_1,x_2,\dotsc,x_k$ 的可能數量，再用這個數量推出數列 $x_1,x_2,\dotsc,x_{k+1}$ 的可能數量。

### 狀態定義
設 $dp_{i, j}$ 為當 $x_i = j$ 時，數列 $x_1,x_2,\dotsc,x_i$ 的可能數量。

### 狀態轉移
假設 $x_i$ 是已知的，那我們只需要往前找 $x_{i-1}$ 為 $x_i-1$、$x_i$、$x_i+1$ 的可能數量，並將其加起來，故此時 $dp_{i, j} = dp_{i - 1, j - 1} + dp_{i-1, j} + dp_{i - 1, j + 1}$。要注意的是，$1 \le j-1,j,j+1 \le m$ 必須成立，所以當 $j=1$ 或 $j=m$ 時要特別處理，我的處理方法是直接將所有 $dp$ 陣列裡 $j=0$ 或 $j=m+1$ 的位子都設為$0$，這樣就不用特別處理了，因為這些位子不管怎麼加進去其他位子都不會改變那個位子的數值。順帶一提，如果 $i=1$ 的話，直接將 $dp_{1, x_1}$ 設 $1$ 就好，因為 $x_1$ 只有一種可能。

如果 $x_{i}$ 是未知的，那我們必須對所有 $dp_{i, j}$ 去計算 $dp_{i, j} = dp_{i - 1, j - 1} + dp_{i - 1, j} + dp_{i - 1, j + 1}$ 其中 $1 \le j \le m$。值得注意的是，當 $i=1$ 時，因為前面還沒有做任何操作，所以要特別抓出來處理，也就是直接將所有的 $dp_{i, j}$（$1 \le j \le m$）設為 $1$。

範例程式碼
---

<details>
<summary>C++ 範例</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
const long long int MOD = 1e9+7;
long long int dp[100010][110];
int main() {
    // ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int n, m;
    cin >> n >> m;
    long long int tmp;
    cin >> tmp;
    if(tmp == 0) {
        for(int j = 1; j <= m; j++) {
            dp[1][j] = 1;
        }
    } else {
        dp[1][tmp] = 1;
    }
    for(int i = 2; i <= n; i++) {
        cin >> tmp;
        if(tmp == 0) {
            for(int j = 1; j <= m; j++) {
                dp[i][j] = ((dp[i - 1][j - 1] + dp[i - 1][j]) % MOD + dp[i - 1][j + 1]) % MOD;
            }
        } else {
            dp[i][tmp] = ((dp[i - 1][tmp - 1] + dp[i - 1][tmp]) % MOD + dp[i - 1][tmp + 1]) % MOD;
        }
    }
    if(tmp == 0) {
        long long int answer = 0;
        for(int i = 1; i <= m; i++) {
            answer = (answer + dp[n][i]) % MOD;
        }
        cout << answer;
    } else {
        cout << dp[n][tmp];
    }
}
```

</details>

滾動最佳化
---
如果你稍微觀察一下，你會發現每次做轉移時，其實我們只會用到 $dp_{i, j}$ 和 $dp_{i + 1, j}$（$0 \le j \le m+1$），$dp_{i - 1, j}$ 之前的東西都不會再使用到了，所以我們可以利用這個想法，把陣列壓縮成 $2\times m$ 大小的陣列，並且使 $dp_{0, j}$ 和 $dp_{1, j}$ 一直互相轉移，覆蓋掉用不到的資料，這個技巧叫做滾動 dp。

範例程式碼
---

<details>
<summary>C++ 範例</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
const long long int MOD = 1e9+7;
long long int dp[2][110];
int main() {
    // ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int n, m;
    cin >> n >> m;
    long long int tmp;
    cin >> tmp;
    if(tmp == 0) {
        for(int j = 1; j <= m; j++) {
            dp[1][j] = 1;
        }
    } else {
        dp[1][tmp] = 1;
    }
    for(int i = 2; i <= n; i++) {
        cin >> tmp;
        for(int j = 1; j <= m; j++) {
            dp[i % 2][j] = 0;
        }
        if(tmp == 0) {
            for(int j = 1; j <= m; j++) {
                dp[i % 2][j]=((dp[(i + 1) % 2][j - 1]+dp[(i + 1) % 2][j]) % MOD + dp[(i + 1) % 2][j + 1]) % MOD;
            }
        } else {
            dp[i % 2][tmp]=((dp[(i + 1) % 2][tmp - 1] + dp[(i + 1) % 2][tmp]) % MOD + dp[(i + 1) % 2][tmp + 1]) % MOD;
        }
    }
    if(tmp == 0) {
        long long int answer = 0;
        for(int i = 1; i <= m; i++) {
            answer = (answer + dp[n % 2][i]) % MOD;
        }
        cout << answer;
    } else {
        cout << dp[n % 2][tmp];
    }
}
```

</details>