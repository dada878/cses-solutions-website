---
sidebar_position: 9
---
Counting Towers
===

題目
---
提供你無限多個長和寬都是整數的方塊，請你建造一個 $2 \times n$ 大小的建築，請問你有多少方法可以蓋這個建築。

只要看起來不同，就算其中一個建築為其他建築的鏡像或旋轉也是不同的。(以下為 4 個不同 $2 \times 2$ 的建築)
![image](https://hackmd.io/_uploads/BJsIqK7XR.png)

## 輸入
- 第一行輸入兩個正整數 $t$。（$1 \le t \le 100$）
- 接下來會有 $t$ 行 ，每一行包含一個數字 $n$。（$1 \le n \le 10^{6}$）

## 輸出
蓋建築的方法數對 $10^9 + 7$ 取餘數

範例測資
---
```
Input : 
2
1
2

Output :
2
8
```
以下為 $2 \times 1$ 建築的兩種蓋法
![image](https://hackmd.io/_uploads/SkYvpn7m0.png)


以下為 $2 \times 2$ 建築的8種蓋法
![image](https://hackmd.io/_uploads/SyANahmQC.png)


想法
---
### 觀察
觀察 $2 \times n$ 的建築，可以發現 $2 \times n$ 的建築分為兩種，最底下分為兩半的建築(建築 a)，最底下連在一起的建築(建築 b)。

![image](https://hackmd.io/_uploads/ryCyza7QA.png)

$2 \times n+1$ 的第 $n + 1$ 層建築有8種可以和 $2 \times n$ 連接的方式

<!-- ![image](https://hackmd.io/_uploads/B1ADMTmQA.png)

![image](https://hackmd.io/_uploads/ryEFzaX7R.png)

![image](https://hackmd.io/_uploads/Bk8qMpQ7C.png)

![image](https://hackmd.io/_uploads/S1ijM6QXA.png)

![image](https://hackmd.io/_uploads/SyjkQa7QR.png)

![image](https://hackmd.io/_uploads/Sy5lma7mC.png)

![image](https://hackmd.io/_uploads/HJxG76QXA.png)

![image](https://hackmd.io/_uploads/B1eX76QXC.png)
 -->

![image](https://hackmd.io/_uploads/S10sNTXmC.png)

如上圖所示
當 $n$ 層為圖a的樣子，且 $n + 1$ 層也是圖a的樣子時，有四種連接方式(圖a1、圖a2、圖a3、圖a4)。
當 $n$ 層為圖a的樣子，且 $n + 1$ 層也是圖b的樣子時，有一種連接方式(圖a6)。
當 $n$ 層為圖b的樣子，且 $n + 1$ 層也是圖a的樣子時，有一種連接方式(圖a5)。
當 $n$ 層為圖b的樣子，且 $n + 1$ 層也是圖b的樣子時，有兩種連接方式(圖a7、圖a8)。

### 狀態定義
$dp_{0, i}$ 為當建築第 $i$ 層為建築 a 的樣子時的建築蓋法。
$dp_{1, i}$ 為當建築第 $i$ 層為建築b的樣子時的建築蓋法。

### 狀態轉移
經由上面觀察得到的東西可以得到
$dp_{0, i} = 4 \times dp_{0, i - 1} + dp_{1, i-1}$
$dp_{1, i} = dp_{0, i - 1} + 2 \times dp_{1, i - 1}$

### 範例程式碼
<details>
<summary> C++ 範例 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
const long long int MOD=1e9+7;
long long int dp[2][1000010];
int main() {
    // ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    dp[0][1]=1;//oo
    dp[1][1]=1;//<>
    for(int i=2;i<=1000000;i++){
        dp[0][i]=(dp[0][i-1]*4+dp[1][i-1])%MOD;
        dp[1][i]=(dp[0][i-1]+dp[1][i-1]*2)%MOD;
    }
    int t;
    cin>>t;
    while(t--){
        long long int n;
        cin>>n;
        cout<<(dp[0][n]+dp[1][n])%MOD<<"\n";
    }
    return 0;
}
```

</details>