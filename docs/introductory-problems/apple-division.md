Apple Division
===

題目
---
有 $n$ 顆蘋果，每個蘋果有重量 $p_i$。你現在要將這些蘋果分成兩堆，使得兩堆蘋果的重量差最小，並輸出該最小重量差。

### 輸入

第一行有一個正整數 $n$

下一行輸入 $n$ 個正整數 $p_i$

- $1 \le n \le 20$
- $1 \le p_i \le 10^9$

### 輸出

輸出一個非負整數，表示最小重量差


解法
---

這題是經典的「枚舉子集」的題目，我們要搜尋所有的可能性，並對於所有可能性去取最小可能的解。

觀察題目會發現到，雖然要將蘋果分成兩堆，但若我們知道其中一堆取的蘋果有哪些，剩下的都會自動被直接放到另一堆。因此只需要考慮其中一堆的重量和即可。

根據排列組合，對於一個大小為 $n$ 的集合，總共有 $2^n$ 種不同的子集（考慮每一種物品是否要取）。因此 $n = 20 \implies 2^{20} \approx 10^6$

這種題目有兩種做法，一種是遞迴的取法，而一種是使用位元運算的做法。

### 1. 遞迴 $O(2^n)$：

維護第一堆的重量 $x$，並依序考慮每一個物品 $i$

對於每一種物品，我們有兩種可能性
1. 將物品放入第一堆
2. 將物品放入第二堆

遞迴下去即可搜尋完所有的可能性

我們可以很簡單地得到以下的程式（0-based）

<details>
<summary>C++ 範例 </summary>
```cpp
#include <bits/stdc++.h>
#define int long long
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
 
using namespace std;
 
int n, sum, ans = 1e18, arr[25];
 
void cal(int now, int val) {
    if(now == n) {
        ans = min(ans, abs(abs(sum - val) - val));
        return;
    }
    cal(now + 1, val + arr[now]);
    cal(now + 1, val);
}
 
signed main() {
    IO;
    cin >> n;
    for(int i = 0; i < n; i++) {
        cin >> arr[i];
        sum += arr[i];
    }
    cal(0, 0);
    cout << ans;
}
```
</details>

### 2. 位元枚舉 $O(n2^n)$

假設我們將選取物品當作是 $1$，而不選當作是 $0$

則我們可以將所有可能性列出來（以下為 $3$ 顆蘋果的可能性）：

$$000, 001, 010, 011, 100, 101, 110, 111$$

會發現這八種其實若看成二進位的形式，會恰好能表示 $0 \sim (2^n-1)$ 的每一個數字，因此，我們可以使用一個 for 迴圈，並使用位元運算的方式去判斷答案。

**Note:** 儘管這個方式比前一個多了一個 $n$，但實際上跑起來的時間不會差太多（遞迴很慢），也可以注意到其實 $n$ 很小，對於整體答案的影響並不大

**Note 2:** 這個問題是 Subset Sum 問題的變種，實際上這個問題為 NP-Complete 的問題，現在是否有足夠快速的演算法仍為未知。但同樣題目的變種「背包問題」則是動態規劃的經典問題，當值域被限制時，可以用更快的方式解決問題

<details>
<summary>C++ 範例 </summary>
```cpp
#include <bits/stdc++.h>
#define int long long
#define IO ios_base::sync_with_stdio(0), cin.tie(0)

using namespace std;

int n, sum, ans = 1e18, arr[25];

signed main() {
    IO;
    cin >> n;
    for(int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    for(int i = 0; i < (1 << n); i++) {
        int fir = 0, sec = 0;
        for(int j = 0; j < n; j++) {
            if(i & (1 << j)) {
                fir += arr[j];
            }
            else {
                sec += arr[j];
            }
        }
        ans = min(ans, abs(fir - sec));
    }
    cout << ans;
}
```
</details>

