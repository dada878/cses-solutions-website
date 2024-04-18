---
sidebar_position: 2
---
Apartments
===

題目
---
有 $n$ 位客人和 $m$ 間公寓。你的任務是為這些客人分配公寓，使得盡可能多的客人都分配到一間公寓。

每個客人都有偏好的公寓大小，客人只會接受任何大小足夠接近偏好（誤差值不大於 $k$）的公寓。

### 輸入
- 第一行有三個正整數 $n$、$m$、$k$。（$1 \le n, m \le 2 \cdot 10^5$，$0 \le k \le 10^9$）
    - $n$ 代表客人數量。
    - $m$ 代表公寓數量。
    - $k$ 代表容許的最大誤差值。
- 第二行有 $n$ 個正整數 $a_i$，分別代表各個客人的偏好大小。（$1 \le a_i \le 10^9$）
    - 若某個客人偏好的大小是 $x$，那他只會接受大小在 $x-k$ 與 $x+k$ 之間的公寓。
- 第三行有 $m$ 個正整數 $b_i$，分別代表各間公寓的大小。（$1 \le b_i \le 10^9$）

### 輸出
輸出共有幾個客人可以分配到公寓。

範例測資
---
```
Input:
4 3 5
60 45 80 60
30 60 75

Output:
2
```

想法 : 雙指針
---
我們先將 $(a_i)$ 與 $(b_i)$ 兩個陣列排序後，運用雙指針的技巧控制兩個索引值 $l$ 與 $r$。當 $a_l$ 可以與 $b_r$ 配對時（也就是當 $|a_l - b_r| \le k$），我們就將 $l$ 與 $r$ 各遞增 $1$；如果不能配對時，我們則判斷如果 $a_l \gt b_r$ 就將 $r$ 遞增 $1$，否則就將 $l$ 遞增 $1$，完成上述操作後就可以繼續進行下一組配對。

### 範例程式碼
<details>
<summary>C++ 範例 </summary>
```cpp
#include <bits/stdc++.h>
using namespace std;

const int maxn = 2e5+5;
int a[maxn], b[maxn];

int main() {
    int n, m, k;
    cin >> n >> m >> k;
    for (int i = 0; i < n; i++)
        cin >> a[i];
    for (int i = 0; i< m; i++)
        cin >> b[i];
    sort(a, a+n);
    sort(b, b+m);

    int l = 0, r = 0, ans = 0;
    while (l < n && r < m) {
        if (abs(a[l]-b[r]) <= k) {
            l++;
            r++;
            ans++;
        } else {
            if (a[l] > b[r])
                r++;
            else
                l++;
        }
    }
    cout << ans;
}
```
</details>