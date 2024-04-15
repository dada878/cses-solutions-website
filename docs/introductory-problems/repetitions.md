Repetitions
===

題目
---
給一個只包含 A、T、C、G 四種大寫英文字母的字串，你的任務是找出最長的疊字，也就是長度最長且只包含一種字母的連續子字串。

### 輸入
一個長度為 $n$ 的字串。（$1 \le n \le 10^6$）

### 輸出
一個整數，表示最長疊字的長度。

範例測資
---
```
Input :
ATTCGGGA

Output :
3
```
最長的疊字是 GGG，長度為 3。

想法
---
遍歷整個字串就能找到最長的疊字長度。

只需要記錄上一個看過的疊字（其實就是上一個字元），這個疊字的長度，以及最長疊字長度即可。如果目前的字元與上一個字元相同，表示目前的字元還在上一個疊字中，疊字長度增加，接著與紀錄中的最長長度做比較；反之開始了新的疊字，疊字的長度變回 $1$。

時間 $O(n)$，空間 $O(1)$。

### 範例程式碼
<details>
<summary>C++ 範例 </summary>
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main() {
    char now;
    char pre = 'X';
    int cnt = 1, ans = 1;
    while (cin >> now) {
        if (now == pre) {
            ++cnt;
            if (cnt > ans) {
                ans = cnt;
            }
        } else {
            cnt = 1;
            pre = now;
        }
    }
    cout << ans;
}
```
</details>
---
回 [「CSES題解集」](https://hackmd.io/MpE3CP9eQUu20n3scMF5Fw?view)