---
sidebar_position: 11
---
Rectangle Cutting
===

## 題目
給兩數 $1\leq a,b\leq500$ ，代表你有一個大小是 $a\times b$ 的長方形，現在你可以切一刀把他切成兩個長方形，問最少要切幾刀才能全部切成正方形。

### 輸入
共一行，兩個數字，代表 $a$ 和 $b$ 。

### 輸出
共一個字，代表最少需要幾刀。

## 範例測資
```
Input : 
3 5

Output :
3
```
切成 $3\times3$ 和 $3\times2$ ，第二刀將 $3\times2$ 切成 $2\times2$ 和 $2\times1$ ，第三刀將 $2\times1$ 切成兩個 $1\times1$ 。
```
o o o|o o
     |
o o o|o o
     |---
o o o|o|o
```

## 想法 1：貪心
每次根據短邊去切出最大的正方形，切完只會剩下一個長方形要切。
但是如果輸入是`5 6`，貪心會切出 $5\times5$ 和 $5$ 個 $1\times5$ ，共五刀。
```
o o o o o|o
         |-
o o o o o|o
         |-
o o o o o|o
         |-
o o o o o|o
         |-
o o o o o|o
```
最佳解則是第一刀切成 $3\times6$ 和 $2\times6$ ，再切成兩個 $3\times3$ 和 $3$ 個 $2\times2$ ，共四刀。
```
o o o|o o o
     |     
o o o|o o o
     |     
o o o|o o o
-----------
o o|o o|o o
   |   |   
o o|o o|o o
```

## 想法 2：試試看全部可能
既然貪心失敗了，或者根本沒去考慮貪心，就先照著題目做做看，我們可以枚舉這一刀要切在哪，並且全部試試看，可以橫著切或直著切。
假設現在是一個 $n\times m$ 的長方形且他的答案是 $dp_{n,m}$ ，若 $n=m$ 答案就是 $0$ ，否則答案是橫著切，也就是 $1+dp_{i,m}+dp_{n-i,m}(1\leq i<n)$ ，或是直著切， $1+dp_{n,j}+dp_{n,m-j}(1\leq j<m)$ ，所有可能之中最小的，所以這其實是一題二維 dp ，枚舉所有可能的長寬，空間複雜度是 $O(nm)$ ，時間複雜度是枚舉不同大小的長方形，以及橫著或直著切在哪，因此時間複雜度是 $O(\sum_{i=1}^n\sum_{j=1}^m(i-1+j-1)=\sum_{i=1}^n(mi+\frac{m(m+1)}{2}-2m)=\frac{mn(n+1)}{2}+\frac{nm(m+1)}{2}-2mn=\frac{nm(n+m-2)}{2})$ ，省略常數就是 $O(nm(n+m))$ 。

### 範例程式碼

<details>
<summary>C++ 範例</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAX 1 << 30
int dp[505][505];
int main(){
    int a, b;
    cin >> a >> b;
    for(int i = 1; i <= a; ++i) {
    	for(int j = 1; j <= b; ++j) {
            if(i == j) {
                dp[i][j] = 0;
                continue;
            }
            dp[i][j] = MAX;
            for(int k = 1;k < i; ++k) dp[i][j] = min(dp[i][j], dp[k][j] + dp[i - k][j] + 1);
            for(int k = 1;k < j; ++k) dp[i][j] = min(dp[i][j], dp[i][k] + dp[i][j - k] + 1);
    	}
    }
    cout << dp[a][b];
}
```

</details>