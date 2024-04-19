---
sidebar_position: 12
---

Palindrome Reorder
===

題目
---
給定一個字串，你的任務是重排字母順序，讓它變成回文字串（正著讀跟倒著讀都一樣）。

### 輸入
一行長度為 $n$ 的字串，只包含「A」到「Z」的字母。（$1 \le n \le 10^6$）

### 輸出
- 如果可以將字串重排成回文字串，將任一種可能的解輸出。
- 如果不可能的話則輸出「NO SOLUTION」。

範例測資
---
```
Input:
AAAACACBA

Output:
AACABACAA
```

`AACABACAA` 倒過來還是 `AACABACAA`

想法
---
首先可以觀察到，如果回文字串長度是奇數，那麼字串中間的字母在整個字串中的出現次數必定為奇數，而其餘字母的出現次數必定為偶數；如果回文字串長度是偶數，則字串中所有字母的出現次數都是偶數。

根據上面的觀察，我們可以根據每個字母的出現次數來判斷有沒有解。如果出現次數為奇數的字母超過一個，那麼它就不能重排成回文字串。否則，它能按照下面的步驟重排成一個回文字串：
1. 計算每個字母 $\alpha$ 的出現次數 $c_\alpha$。
2. 每個字母 $\alpha$ 都先列出 $\lfloor c_\alpha/2 \rfloor$ 次。
3. 列出唯一一個出現次數為奇數的字母。（若沒有就忽略）
4. 按照 2. 的順序倒過來列出。

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
using namespace std;

string s, odd = "";
map<char, int> cnt;
stack<char> stk;
int main() {
    cin >> s;
    for (char i : s)
        cnt[i]++;
    for (auto i : cnt) {
        if (i.second & 1)
            odd += i.first;
    }
    if (odd.size() > 1)
        cout << "NO SOLUTION";
    else {
        for (auto i : cnt) {
            for (int j = 0; j < i.second/2; j++)
                cout << i.first;
            stk.push(i.first);
        }
        cout << odd;
        while (!stk.empty()) {
            for (int j = 0; j < cnt[stk.top()]/2; j++)
                cout << stk.top();
            stk.pop();
        }
    }
}
```
</details>
