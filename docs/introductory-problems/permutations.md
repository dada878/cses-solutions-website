---
sidebar_position: 5
---

Permutations 
===

題目
---
如果一個 $n$ 排列相鄰兩項之差都不是 $1$，那我們就說這個排列是漂亮的。

> 一個 $n$ 排列就是一個由 $1$ 到 $n$ 組成的序列，每個數字既不重複也不缺漏。

給定 $n$，如果存在一個漂亮的 $n$ 排列則將它建構出來。

### 輸入
一個整數 $n$。（$1 \le n \le 10^6$）

### 輸出
輸出一個漂亮的 $n$ 排列。
若有很多個解，只要輸出其中任何一個。
若沒有解，則輸出「NO SOLUTION」（不含引號）

範例測資
---
```
Input1:
5

Output1:
4 2 5 3 1

Input2:
3

Output2:
NO SOLUTION
```

想法 1：產生排列並檢查是否滿足條件（TLE）
--- 
### 想法 1-1：用遞迴產生排列

在一個 $n$ 排列中，整數 $1$ 到 $n$ 都恰好出現一次。如果我們從第一項開始建構排列，那麼每次決定一個數字都會影響到後面項的選擇：如果某一項選了 $k$，那麼它之後的每一項都不能選 $k$。我們可以用遞迴來產生這些所有的排列。

但我們只需要輸出一個滿足條件的排列就好，所以當我們遇到符合條件的排列就不再繼續產生其他排列。

不幸的是，這樣寫會 TLE。

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
#define maxn 1000005
using namespace std;

int p[maxn];
bool is_beautiful(int n) {
    for (int i = 1; i < n; i++)
        if (abs(p[i] - p[i-1]) == 1)
            return false;
    return true;
}

bool used[maxn] = {false};
bool beautiful_perm(int n, int i){
    if (i == n) return is_beautiful(n);
    for (int k = 1; k <= n; k++) {
        if (used[k]) continue;
        p[i] = k;
        used[k] = true;
        if (beautiful_perm(n, i+1)) return true;
        used[k] = false;
    }
    return false;
}
int main(){
    int n;
    cin >> n;
    if (beautiful_perm(n, 0)) {
        for (int i = 0; i < n; i++) {
            cout << p[i] << ' ';
        }
    } else {
        cout << "NO SOLUTION";
    }
}
```
</details>

### 想法 1-2：用 `next_permutation` 產生排列
C++ 的[標準函式庫](https://en.cppreference.com/w/cpp/algorithm/next_permutation)有內建 `next_permutation`。

對於一個序列 `s`，我們可以將恰好包含所有 `s` 元素的所有排列按照字典序做排序。如果在這個排序上存在 `s` 的下一個排列，那麼 `next_permutation(s.begin(), s.end())` 就會將 `s` 原地重排成那個排列並回傳 `true`；否則就將 `s` 原地重排成第一個排列並回傳 `false`。每次呼叫的時間複雜度都是 $O(n)$。

> `next_permutation` 的具體實作可以參考 LeetCode 第 31 題。

這只是產生排列這件事更加輕鬆而已，依然會 TLE。

### 範例程式碼
前面和上一個程式相同，有定義 `int p[]` 與 `bool is_beautiful(int n)`。
<details>
<summary>C++ 範例</summary>
```cpp
int main(){
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        p[i] = i + 1;
    }
    bool found = is_beautiful(n);
    while (!found && next_permutation(p, p+n)) {
        found = is_beautiful(n);
    }
    if (found) {
        for (int i = 0; i < n; i++) {
            cout << p[i] << ' ';
        }
    } else {
        cout << "NO SOLUTION";
    }
}
```
</details>

### 想法 1-3：用遞迴按照條件產生排列
上述兩種方法其實都是按照字典序產生排列。而且我們很快地就會發現對於 $n$ 排列來說，前 $(n-2)!$ 個排列一定都是 $1\ 2$ 以開頭的，必定不符合題目條件。

> 比如說當 $n = 10$ 時，以 $1\ 2$ 開頭的排列有 $8! = 40320$ 個。而第一個滿足條件的排列是 $1\ 3\ 5\ 2\ 4\ 6\ 8\ 10\ 7\ 9$，是字典序中第 $50411$ 個排列。

不過我們並不需要建構完一整個排列之後才去檢查它有沒有符合條件，而是在建構排列的途中就可以先用條件篩選每一項要選擇的數字。這樣一來我們除了事後不必再檢查條件，還能省去大量的冤枉路。

雖然跳過了許多明顯失敗的排列，但依然會 TLE。

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
#define maxn 1000005
using namespace std;

int p[maxn];
bool used[maxn] = {false};
bool beautiful_perm(int n, int i){
    if (i == n) return true;
    for (int k = 1; k <= n; k++) {
        if (used[k]) continue;
        if (abs(p[i-1] - k) == 1) continue;
        p[i] = k;
        used[k] = true;
        if (beautiful_perm(n, i+1)) return true;
        used[k] = false;
    }
    return false;
}
int main(){
    int n;
    cin >> n;
    if (beautiful_perm(n, 0)) {
        for (int i = 0; i < n; i++) {
            cout << p[i] << ' ';
        }
    } else {
        cout << "NO SOLUTION";
    }
}
```
</details>

想法 2：直接生成相鄰兩項不相差 1 的排列
---
其實這題有非常直接的建構法。

先將整個排列分為一串只有奇數的排列和一串只有偶數的排列，最後再將它們拼在一起。
因為兩個奇數必定相差 $2$ 以上，兩個偶數也是如此，接著只需要注意拼接處也不差 $1$ 就好。要達成這個條件我們可以讓兩個排列都遞增或都遞減。

> 要特別注意 $n = 4$ 的時候會不會接壞。

透過上面的建構法可以觀察到，事實上只有 $n=2$ 或 $n=3$ 時構建不出符合條件的排列，特別將這兩種情況判斷出來處理即可。

這樣直接的建構法時間複雜度只有 $O(n)$，空間複雜度也只有 $O(1)$。

### 範例程式碼
<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;
    if (n == 1) {
        cout << '1';
    } else if (n < 4) {
        cout << "NO SOLUTION";
    } else {
        for (int i = 2; i <= n; i += 2) {
            cout << i << ' ';
        }
        for (int i = 1; i <= n; i += 2) {
            cout << i << ' ';
        }
    }
}
```
</details> 