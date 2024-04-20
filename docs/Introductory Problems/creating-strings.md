---
sidebar_position: 15
---

Creating Strings
===

題目
---
給你一個字串，請輸出所有可以用這些字母所構成的字串。

### 輸入

一個由 a-z 組成的字串，長度為 $n$

- $1 \le n \le 8$

### 輸出

在第一行輸出一個數字 $k$，表示有幾種相異的字串組合。接下來的 $k$ 行，每行輸出一個字串，必須以字典序順序輸出。


範例測資
---
```
Input :
aabac

Output :
20
aaabc
aaacb
aabac
aabca
aacab
aacba
abaac
abaca
abcaa
acaab
acaba
acbaa
baaac
baaca
bacaa
bcaaa
caaab
caaba
cabaa
cbaaa
```

想法
---

使用 C++ 內建的函式 `next_permutation` 即可解決

**Note 1 :** C++ 的 `string` 其實就是 `vector<char>` 的進化版
**Note 2 :** `do while` 跟 `while` 的差別在於，`do while` 會先執行回圈內的程式，再判斷是否符合條件
**Note 3 :** `next_permutation` 是一個枚舉函式，他會根據字典序從小到大枚舉每一種排列，因此在使用前必須先將東西排序好才能枚舉到每一種情況

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
 
#define ll long long
#define fastio ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
 
using namespace std;
 
int main() {
    fastio
    vector<string> v;
    string s;
    cin >> s;
    sort(s.begin(), s.end());
    do{
        v.push_back(s);
    } while(next_permutation(s.begin(), s.end()));
    cout << v.size() << "\n";
    for(auto x : v){
        cout << x << "\n";
    }
}

```
</details>