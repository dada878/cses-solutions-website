Missing Number
===

題目
---
給你 $1$ 到 $n$ 之間所有的整數，但少了一個數字。你的任務是找出缺少了哪一個數字。

### 輸入
- 第一行輸入一個整數 $n$。（$2 \le n \le 2 \cdot 10^5$）
- 第二行輸入 $n-1$ 個不重複的整數 $k_i$。（$1 \le k_i \le n$）

### 輸出
輸出缺少的數字

範例測資
---
```
Input : 
5
2 3 1 5

Output :
4
```

輸入為 $2$ $3$ $1$ $5$ ，在 $1$ 到 $5$ 的中少了 $4$，因此要輸出 $4$。

想法 1：排序後檢查
---
先把輸入的數字排序過後，循序找解即可。

複雜度是 $O(n\log n+n)$。

### 範例程式碼

<details> 
<summary>C++範例</summary>
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        int n, x;
        vector<int> xs;
        cin >> n;
        for (int i = 0; i < n - 1; i++) {
            cin >> x;
            xs.push_back(x);
        }
        sort(xs.begin(), xs.end());
        for (int i = 0; i < n; i++) {
            if(xs[i] != i + 1) {
                cout << i + 1;
                break;
            }
        }
    }
    ```
</details>

想法 2：計數檢查
---
我們可以先建立長度為 $n$ 的列表來記錄個別數字出現的次數（在這題要不是 $0$ 就是 $1$），之後遍歷整個列表找哪個數字出現 $0$ 次就好。

### 範例程式碼

<details>
<summary>C++ 範例 </summary>
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        int n, x;
        cin >> n;
        vector<int> cnt(n, 0);
        for (int i = 0; i < n - 1; i++) {
            cin >> x;
            cnt[x-1] = 1;
        }
        for (int i = 0; i < n; i++) {
            if(cnt[i] == 0) {
                cout << i + 1;
                break;
            }
        }
    }   
    ```
</details>

想法 3：從總和檢查
---
考慮一個有結合律與交換律的「加法」運算 $\oplus$，那麼輸入就可以不斷用 $\oplus$ 來「加總」成 $
  1 \oplus 2 \oplus \cdots \oplus (k-1) \oplus (k+1) \oplus \cdots \oplus n,
$ 其中 $k$ 是缺少的數字。如果這個 $\oplus$ 運算是可逆的，那我們就能拿完整 $1 \oplus 2 \oplus \cdots \oplus n$ 的結果「減去」上面的「總和」，得到缺少的 $k$。

基本上需要 $O(n)$ 的時間與 $O(1)$ 的空間來計算「總和」。

下面依照選用的運算 $\oplus$，提供兩種不同的方法。

### 想法 3-1：加法版
使用加法。其中我們有等差級數公式 $1 + 2 + \dots + n = \dfrac{n(n+1)}{2}$。

但要注意 $n$ 足夠大的時候，總和會超過 `int` 的範圍，必須使用 `long long` 才能通過 CSES 的測資。

> 或者你可以在每次運算後都 mod 比 $n$ 大的數。

### 範例程式碼

<details>
<summary>C++ 範例 </summary>
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        long long n, x;
        long long sum = 0;
        cin >> n;
        for (int i = 0; i < n - 1; i++) {
            cin >> x;
            sum += x;
        }
        cout << (n * (n + 1) / 1) - sum;
    }
    ```
</details>

### 想法 3-2：XOR 版
令 $a \oplus b$ 為 $a$ 與 $b$ 經過 bitwise XOR（也就是程式上的 `a ^ b`）之後的運算結果，這個運算也是滿足結合律與交換律的。並且任何整數的反元素就是自己：$a \oplus b \oplus b = a$，換句話說，「減去」跟「加上」是同樣的運算。

另外，觀察可得 $4k \oplus (4k+1) \oplus (4k+2) \oplus (4k+3) = 0$，於是就有$
  1 \oplus 2 \oplus \cdots \oplus n
  = (4 \lfloor n/4 \rfloor) \oplus \cdots \oplus n,
$其中 $4 \lfloor n/4 \rfloor$ 可以用 `n >> 2 << 2` 算得。

這個方法相較於上一個的方法好的地方在於總和一定比 $2n$ 來得小，較不用擔心太大的 $n$ 會超出整數範圍。

> 下面的過程中都是位元運算，若加上 io 優化這題甚至能做成 n 到 8e18 的鴨腸題。

### 範例程式碼

<details>
<summary>C++ 範例 </summary>
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        int n, x;
        int all = 0, sum = 0;
        cin >> n;
        for (int i = 0; i < n - 1; i++) {
            cin >> x;
            sum ^= x;
        }
        for (int i = n >> 2 << 2; i <= n; i++) {
            all ^= i;
        }
        cout << (all ^ sum);
    }
    ```
<details>
