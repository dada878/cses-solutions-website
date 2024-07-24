---
sidebar_position: 12
---

Money Sums
===

## 題目

給你幾個硬幣，請問你可以組出多少不同的價格

### 輸入
- 第一行給定一個 $n$ ($1 \le n \le 100$)
- 第二行給定 $n$ 個數字 $x$ ($1 \le x_i \le 1000$)

### 輸出
- 第一行輸出一個數字 $k$，代表可以組出不同價格的數量
- 第二行依照小到大把所有不同的價格輸出出來
## 範例測資
```
Input:
4
4 2 5 2

Output:
9
2 4 5 6 7 8 9 11 13
```

總共 9 種可能
- $2$
- $4$
- $5$
- $2 + 4$
- $2 + 5$
- $2 + 2 + 4$
- $4 + 5$
- $2 + 4 + 5$
- $2 + 2 + 4 + 5$

## 觀察

觀察輸出有兩個重點
- 他要輸出所有不同的價格
- 它的價格要從小排到大
    
## 想法

根據觀察的兩個重點，可以直接砸 set。  
當執行第 $i$ 輪的時候，針對第 $i-1$ 輪的所有數字加上 $x$ 後加進去 set。
最後得到的結果就是答案。

因為n最多為 $100$，且 $x$ 最多為 $1000$。
所以總價格裡面最高為 $10^5$。
也就是說，set 裡面最多塞 $10^5$ 個數字。
且你要執行 $n$ 輪，所以說時間複雜度是 $O(nlog(10^5))$

## 程式

<details>
<summary>C++ 範例</summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
set<int> num;
int main(){
    int n;
    cin>>n;
    num.insert(0);
    for(int i=0;i<n;i++){
        int a;
        cin>>a;
        vector<int> tmp;
        for(auto b:num){
            tmp.push_back(b+a);
        }
        for(auto b:tmp){
            num.insert(b);
        }
    }
    cout<<num.size()-1<<"\n";
    int i=-1;
    for(auto a:num){
        i++;
        if(i==0)continue;
        if(i!=1)cout<<" ";
        cout<<a;
    }
    return 0;
}
```

</details>