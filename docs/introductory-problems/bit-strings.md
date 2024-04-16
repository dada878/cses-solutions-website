---
sidebar_position: 9
---

Bit Strings
===

題目
---
你的任務是算出長度為 $n$ 的位元字串有多少個。

例如當 $n = 3$，那答案就是 $8$，因為所有長度為 $3$ 的位元字串分別是 `000`、`001`、`010`、`011`、`100`、`101`、`110`、`111`。

### 輸入
一個正整數 $n$。（$1 \le n \le 10^6$）

### 輸出
輸出答案（長度 $n$ 的位元字串數量）模 $10^9+7$。


範例測資
---

```
Input :
3

Output :
8
```

想法：快速冪
---
字串中的每個字元都是 `0` 或 `1` 兩種二選一，所以長度 $n$ 的位元字串就總共有 $2^n$ 種。那麼這題只要輸出 $2^n \bmod (10^9+7)$ 即可。

我們可以用俗稱「快速冪」的算法快速算出 $2^n$，這個算法只有 $O(\log n)$ 的時間複雜度，比起 $O(n)$ 的直接連乘法快上許多。

考慮一個滿足結合律的「乘法」運算 $\otimes$，並且把 $n$ 個 $x$「乘」在一起用 $x^n$ 表示。因為「乘法」滿足結合律，所以「次方」也會滿足指數律：$
  x^{m+n} = x^m \otimes x^n,\quad
  x^{mn} = (x^m)^n.
$ 快速冪的核心概念就是用指數律來降低「乘法」的次數。我們通常會用 $2$ 來除，比如說把 $x^{14}$ 變成 $(x^7)^2$ 或 $(x^2)^7$。像這樣一直遞迴下去，我們就能用 $O(\log n)$ 次的「乘法」算出 $x^n$。

注意到我們有以下的事實：$
  (a \times b) \bmod m = ((a \bmod m) \times (b \bmod m)) \bmod m.
$ 如果我們進一步把 $a \otimes b$ 定義成 $(a \times b) \bmod m$，我們還可以驗證說這個運算的確滿足結合律。所以我們可以直接把取模寫進快速冪裡面，而不用整個算完再取模。（而且如果整個算完，數字可能會大到存不下）

因為快速冪有不少種寫法，下面的範例程式碼先省略不定義 `fastpow`，後面再把幾種寫法列出來。

### 範例程式碼
<details>
<summary>C++ 範例 </summary>
```cpp
#include <iostream>
using namespace std;

int MOD = 1000000007;
int fastpow(int x, int n);

int main() {
    int n;
    cin >> n;
    cout << fastpow(2, n);
}
```
</details>

### 快速冪 1：$x^{2k} = (x^k)^2$
例如 $x^{14} = ((x^2 \otimes x)^2 \otimes x)^2$。

這裡使用以下的規則：$
  x^n = \begin{cases}
    1, & n = 0;\\
    x^k \otimes x^k, & n = 2k;\\
    x^k \otimes x^k \otimes x, & n = 2k+1.
  \end{cases}
$ 並使用遞迴來實現。

> 如果沒有「乘法」單位元素 $x^0$，要把初始條件改成 $n = 1$ 時回傳 $x$。

<details>
<summary>C++ 範例</summary>
```cpp
int fastpow(int x, int n) {
    if (n == 0)
        return 1;
    int u = fastpow(x, n/2);
    u = (u * u) % MOD;
    if (n % 2)
        u = (u * x) % MOD;
    return u;
}
```
</details>
### 快速冪 2：$x^{2k} = (x^2)^k$
例如 $x^{14} = x^8 \otimes x^4 \otimes x^2$，其中 $x^{2n}$ 都是從已經算過的 $x^n$「平方」之後算出來的。

這裡使用以下的規則： $
  x^n = \begin{cases}
    1, & n = 0;\\
    (x \otimes x)^k, & n = 2k;\\
    (x \otimes x)^k \otimes x, & n = 2k+1.
  \end{cases}
$ 並使用遞迴來實現。

> 如果沒有「乘法」單位元素 $x^0$，要把初始條件改成 $n = 1$ 時回傳 $x$。
<details>
<summary>C++ 範例</summary>
```cpp
int fastpow(int x, int n) {
    if (n == 0)
        return 1;
    int u = fastpow((x * x) % MOD, n/2);
    if (n % 2)
        u = (u * x) % MOD;
    return u;
}
```
</details>

我們也可以寫成迴圈。這個順序恰好跟遞迴版本顛倒，以 $x^7$ 為例，上面的遞迴版會算 $x^4 \otimes x^2 \otimes x$，而下面的迴圈版是算 $x \otimes x^2 \otimes x^4$。

<details>
<summary>C++ 範例 </summary>
```cpp
int fastpow(int x, int n) {
    int r = 1;
    while (n) {
        if (n % 2)
            r = (r * x) % MOD;
        x = (x * x) % MOD;
        n /= 2;
    }
    return u;
}
```
</details>
