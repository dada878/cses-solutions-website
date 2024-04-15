Distinct Numbers
===

題目
---
給一個整數陣列，你的任務是計算這個陣列之中相異整數的數量。

### 輸入
- 第一行有一個整數 $n$。（$1 \le n \le 2 \cdot 10^5$）
- 第二行有 $n$ 個整數 $x_i$。（$x_i \le 10^9$）

### 輸出
輸出相異整數的數量。

範例測資
---

```
Input:
5
2 3 2 2 3

Output:
2
```

想法 : 資料結構 set
---
利用 set 不重複元素的特性來記錄已經出現過的數字，最後輸出 set 的大小即為所求。

### 範例程式碼

<details>
    
<summary>C++ 範例</summary>
    
    ```cpp
    
    #include <bits/stdc++.h>
    using namespace std;
 
    set<int> nums;

    int main() {
        int n, x;
        cin >> n;
        while (n--) {
            cin >> x;
            nums.insert(x);
        }
        cout << nums.size();
    }
    ```
    
</details>
