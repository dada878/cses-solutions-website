---
sidebar_position: 13
---

Gray Code
===

題目
---
格雷碼是一長度為 $2^n$ 的二進位數列，其中每項長度都是 $n$ ，並且滿足條件 : 任意兩相鄰的數字只有一個位元改變。請根據輸入的 $n$ 輸出格雷碼。

### 輸入
輸入一個正整數 $n$。 ($1 \le n \le 16$)

### 輸出
輸出 $2^n$ 個數字並且符合格雷碼的規則，可以輸出任一一組合法的解。

範例測資
---
```
Input:
2

Output:
00
01
11
10
```

$00$ $\rightarrow$ $01$ 右邊的 $0$ 變成 $1$

$01$ $\rightarrow$ $11$ 左邊的 $0$ 變成 $1$

$11$ $\rightarrow$ $10$ 右邊的 $1$ 變成 $0$

## 解法

觀察到當 $n = 1$ 時，答案為 $0, 1$

而當 $n = 2$ 時，其中一組解為 $00, 10, 11, 01$

**Claim: 令 Gray(n) 為合法的長度 $n$ 格雷碼陣列，則 Gray(n) 可以由 $Gray(n-1) + \text{Reverse}(Gray(n-1))$ 每一個元素複製兩次並在後面依序接上 $0$ 與 $1$ 所構成** 

**Proof.** ~~Trivial~~，因為已知 Gray(n-1) 滿足性質，在後面以遞迴方式去加數字並不會影響相鄰元素相差一個位元的條件

<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<string>vt;
int main() {
    cin >> n;
    vt.push_back("0");
    vt.push_back("1");
    for(int i = 2; i <= n; i++) {
        int sz = vt.size();
        for(int j = sz - 1; j >= 0; j--) vt.push_back(vt[j]);
        for(int j = 0; j < sz; j++) vt[j] = "0" + vt[j];
        for(int j = sz; j < sz * 2; j++) vt[j] = "1" + vt[j];
    }
    for(auto i : vt) cout << i << endl;
}
```
</details>

<details>
<summary> C++ 遞迴解法 by sam571128</summary>
```cpp
#include <bits/stdc++.h>
 
#define int long long
#define fastio ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
 
using namespace std;
 
void generateGrayarr(int n)
{
    // base case
    if (n <= 0)
        return;
 
    // 'arr' will store all generated codes
    vector<string> arr;
 
    // start with one-bit pattern
    arr.push_back("0");
    arr.push_back("1");
 
    // Every iteration of this loop generates 2*i codes from previously
    // generated i codes.
    int i, j;
    for (i = 2; i < (1<<n); i = i<<1)
    {
        // Enter the prviously generated codes again in arr[] in reverse
        // order. Nor arr[] has double number of codes.
        for (j = i-1 ; j >= 0 ; j--)
            arr.push_back(arr[j]);
 
        // append 0 to the first half
        for (j = 0 ; j < i ; j++)
            arr[j] = "0" + arr[j];
 
        // append 1 to the second half
        for (j = i ; j < 2*i ; j++)
            arr[j] = "1" + arr[j];
    }
 
    // print contents of arr[]
    for (i = 0 ; i < arr.size() ; i++ )
        cout << arr[i] << "\n";
}
 
signed main(){
	fastio
	int n;
	cin >> n;
	generateGrayarr(n);
}
```
</details>


