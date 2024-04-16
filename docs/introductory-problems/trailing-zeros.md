Trailing Zeros
===

題目
---

你的任務是計算 $n$ 階乘的後綴 $0$ 數量。
例如：$20! = 2432902008176640000$，它尾隨著 $4$ 個 $0$。


### 輸入
一個整數 $n$（$1 \le n \le 10^9$）。

### 輸出
一個整數，表示 $n!$ 的後綴 0 數量。

範例測資
---

Input:
```
20
```
Output:
```
4
```

想法
---

在 $n!$ 的值中，尾隨的 0 是由因數 10 所產生，可將其分解為因數 2 和 5，由於 $n! = 1 \times 2 \times \dots \times n$，在這之中每個偶數都會提供一個因數 2，而因數 5 則相對稀少，所以我們只要算出 $n!$ 中有幾個因數 5，必定可以與其他的因數 2 湊成一個因數 10 進而讓最終值產生一個後綴 0。

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n, ans = 0; 
    cin >> n;
    while(n) {
        ans += n / 5;
        n /= 5;
    }
    cout << ans << '\n';
}
```
</details>