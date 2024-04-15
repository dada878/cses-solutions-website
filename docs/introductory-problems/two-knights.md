Two Knights 
===

題目
---

在一個 $k × k$ 的棋盤上放置兩個騎士，要求它們不互相攻擊。
我們要計算出當 $k = 1, 2, ..., n$ 時，有多少種符合要求的放置方式。


### 輸入
一個整數 $n$（$1 \le n \le 10^4$）。

### 輸出
$n$ 個整數，表示 $k = 1, 2, ..., n$ 時的放置方法數。

範例測資
---

Input:
```
8
```
Output:
```
0
6
28
96
252
550
1056
1848
```

想法
---

題目要求兩騎士不互相攻擊，首先我們要知道騎士是採用 L 形斜角的攻擊方式，如下圖（[圖片來源](https://www.google.com/url?sa=i&url=http%3A%2F%2Fchiuinan.github.io%2Fgame%2Fgame%2Fintro%2Fch%2Fc41%2Fchess2%2Fchess_rule%2Fchess_rule.htm&psig=AOvVaw38InAo20FMMBlAEOGZxKji&ust=1712492990430000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCNDmq93LrYUDFQAAAAAdAAAAABAE)）。
![image](https://hackmd.io/_uploads/rJuDeTCJA.png)

題目不是要求擺放位置而是放置方法數，所以

**合法的放置方法數 = 棋盤的總放置方法數 - 不合法的放置方法數**

- 棋盤的總放置方法數
表示兩騎士可隨意放置且不重複的方法數，
為 **第一個騎士可擺放的位置數 * 第二個騎士可擺放的位置數 / 2**
$$
\frac{(k \times k) \times (k \times k - 1)}{2}
$$

- 不合法的放置方法數
表示兩騎士可互相攻擊的放置方法數
分為下圖 4 個種類，並依照邊長排滿整個棋盤
$$
(k - 2) \times (k - 1) \times 4
$$
![image](https://hackmd.io/_uploads/rJepQp0kR.png)

綜合上述，對於 $k = 1, 2, ..., n$，合法的放置方法數為

$$
\frac{(k \times k) \times (k \times k - 1)}{2} - (k - 2) \times (k - 1) \times 4
$$

複雜度：時間 $O(n)$，空間 $O(1)$。

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    unsigned long long n, k = 1;
    for (cin >> n; k <= n; ++k){
        cout << (k * k * (k * k - 1) / 2) - ((k - 2) * (k - 1) * 4) << '\n';
    }
}
```
</details>
