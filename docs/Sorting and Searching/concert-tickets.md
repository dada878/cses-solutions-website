Concert Tickets
===

題目
---
有 $n$ 張演唱會門票，每張票都有各自的價格，接著有 $m$ 位客人一個接一個的來。
每個客人都有自己願意付的價格，他們會買所有票裡面小於等於自己願付價格，且價格最高的那張票。

### 輸入
- 第一行有兩個正整數 $n$ 和 $m$ ，代表票的數量跟客人的數量。 $(1 \le n, m \le 2 \cdot 10^5)$
- 第二行有 $n$ 個整數 $h_i$，分別代表每張票的價格。$(1 \le h_i \le 10^9)$
- 第三行有 $m$ 個整數 $t_i$，分別代表每個客人的願付價格。$(1 \le t_i \le 10^9)$

### 輸出
每個客人分別會花多少錢買票，如果沒有可以買的票則輸出 $-1$。

範例測資
---
```
Input:
5 3
5 3 7 8 5
4 8 3

Output:
3
8
-1
```

想法
---
每位客人都要找小於或等於某個價格的票，要怎麼最快找到符合條件的票呢 ? 二分搜是我們最好的選擇，因為排序後的票價具有單調性。最後的演算法是使用二分搜找到當前客人要買的票，並且將其從未售出的票裡面移除，因為移除不影響原序列單調性，下一位客人需要的票也可以直接使用二分搜找到答案。

直接實作好麻煩喔，有什麼比較方便的方法實作這題嗎 ? C++有內建資料結構 `multiset`，他會將已加入的元素進行排序，也可以從中移除元素，並且加入和移除元素的時間複雜度皆為 $O(logn)$。

**Note :** `upper_bound` 是 C++ 內建的二分搜函式，他會回傳第一個大於目標值的迭代器，因此在 `upper_bound` 的前一個位子就是要找的答案，若 `upper_bound` 回傳的迭代器是 `multiset` 的第一個位子，代表沒有小於等於顧客需求的票價，輸出 $-1$ 。

### 範例程式碼

<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
#define int long long 
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
using namespace std;

signed main() {
    IO;
    int n, m;
    cin >> n >> m;
    multiset<int>mst;
    for(int i = 0; i < n; i++) {
        int h;
        cin >> h;
        mst.insert(h);
    }
    for(int i = 0; i < m; i++) {
        int t;
        cin >> t;
        auto it = mst.upper_bound(t);
        if(it == mst.begin()) {
            cout << -1 << endl;
        }
        else {
            it--;
            cout << *it << endl;
            mst.erase(it);
        }
    }
}
```
</details>