---
sidebar_position: 8
---

Two Sets
===

題目
---
將 $1$ 到 $n$ 的所有正整數分成兩組，使兩集合的總和相等。

### 輸入
正整數 $n$

### 輸出
- 如果有可能達成題目條件，輸出 "Yes" ，否則輸出 "No"
- 如果輸出 "Yes" ，輸出第一組集合的大小，並在下一行輸出該集合內的所有數字，第二個集合也用相同的方式輸出

範例測資
---
```
Input :
7

Output :
YES
4
1 2 4 7
3
3 5 6
```

解法一
---
觀察 : 
一 、 ==任意四個連續的整數可拆成 $n$, $n + 2$ 以及 $n + 1, n + 2$ 兩個總和相等的數組==。
二 、 ==$1$ 到 $3$ 可拆成 $1$, $2$ 以及 $3$ 兩組==。

如果 $n$ 是 $4$ 的倍數，可藉由上面觀察的第一點去分組，如果 $n$ 是 $4$ 的倍數加 $3$ ，則將 $1$ 到 $3$ 由觀察的第二點先處理好，剩下再以第一點做處理


### 範例程式碼
<details>
<summary>C++ 範例  </summary>
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n;
    cin >> n;
    if(n < 3) cout << "NO";
    else if(n == 3) cout << "YES" << endl << 1 << endl << 3 << endl << 2 << endl << 1 << ' ' << 2;
    else if(n % 4 == 0) {
        cout << "YES";
        int k = 2;
        int i = 1;
        int j = n;
        while(k--) {
            cout << endl << n/2 << endl;
            for(int p = 0; p < n / 4; p++) {
                cout << i << ' ' << j << ' ';
                i++, j--;
            }
        }
    } 
    else if(n % 4 == 3) {
        cout << "YES" << endl;
        set<int>f;
        set<int>s;
        f.insert(1);
        f.insert(2);
        s.insert(3);
        for(int i = 4;i <= n; i++) {
            if(i % 4 == 0) f.insert(i);
            else if(i % 4 == 3) f.insert(i);
            else if(i % 4 == 1) s.insert(i);
            else s.insert(i);
        }
        cout << f.size() << endl;
        for(auto i = f.begin(); i != f.end(); i++) cout << *i << ' ';
        cout << endl << s.size() << endl;
        for(auto i = s.begin(); i != s.end(); i++) cout<<*i<<' ';
    }
    else cout<<"NO";
}
```
</details>

解法二
---
#### Claim: $\{1, 2, 3, \cdots, n\}$ 可以組成 $1 \sim \frac{n(n+1)}{2}$ 之間的任意數字 $x$。同時，若以貪心法（每次選取最大 $\le x$ 的數字去湊）一定可以組成 $x$

#### Proof: 

對 $n$ 作數學歸納法：

Base Case: 當 $n=1$ 時，顯然滿足

Inductive Step: 假設對於 $n$，我們可以使用 $1 \sim n$ 的數字組成 $[1,\frac{n(n+1)}{2}]$ 中的任意數字。則對於 $x \in [\frac{n(n+1)}{2} ,\frac{(n+1)((n+1)+1)}{2}]$，取 $k = x - (n+1) \in [1,\frac{n(n+1)}{2}]$，則我們可以構成任意 $x \in [\frac{n(n+1)}{2} ,\frac{(n+1)((n+1)+1)}{2}]$。

由歸納法之步驟，也可以相同的方式構造出一組取法。

### 作法：

根據上面的 Claim，若 $\frac{n(n+1)}{2}$ 為整數，則我們可以取 $x = \frac{n(n+1)}{4}$，並用貪心法取出一組總和為 $x$ 的答案。令該集合為 $S$，則 $\{1,2,\cdots,n\} - S$ 即為另外一組總和為 $x$ 的取法

<details>
<summary>C++ 範例</summary>
```cpp
#include <bits/stdc++.h>
 
#define ll long long
#define fastio ios_base::sync_with_stdio(0); cin.tie(0); 
 
using namespace std;
 
int main(){
	fastio
	ll int n, sum1 = 0,sum2 = 0;
	unordered_map<int,int> m;
	cin >> n;
	if((n*(n+1)/2)&1) cout << "NO" << "\n";
	else {
		cout << "YES\n";
		ll target = n * (n + 1) / 2 / 2;
		vector<int>ans1;
		for(int i = n;i >= 1;i--){
			if(target - sum1 > i) {
				sum1 += i, m[i]++;
				ans1.push_back(i);
			}
			else{
				ans1.push_back(target-sum1);
				m[target-sum1]++;
				sum1 += target-sum1;
				break;
			}
		}
		cout << ans1.size() << "\n";
		for(auto x : ans1) cout << x << " ";
		cout << "\n" << n - ans1.size() << "\n";
		for(int i = 1;i <= n;i++){
			if(!m[i]) cout << i << " ";
		}
	}
}
```
</details>