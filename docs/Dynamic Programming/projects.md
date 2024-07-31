---
sidebar_position: 16
---

Projects
===

## 題目
給一個數 $n$ $(1\leq n\leq2\times10^5)$ 代表有 $n$ 個專案能做，
接著有 $n$ 行，每行第一個數代表專案開始的日期，第二個數代表結束的日期，第三個數代表能賺到的錢。

一天只能同時做一個專案（開始日期和結束日期完全不能重疊），
且一個專案要完全做完才能拿到錢（不能只做部分），
問最多能賺多少錢。

### 輸入
第一行一個數 $n$ ，接著有 $n$ 行，每行三個數，第一個是開始日期，第二個是結束日期，第三個是能賺到的錢 $(1\leq開始日期\leq結束日期\leq10^9,1\leq能賺到的錢\leq10^9)$ 。

### 輸出
一個數，代表最多能賺多少錢。

範例測資
---
```
Input:
4
2 4 4
3 6 6
6 8 2
5 7 3

Output:
7
```

想法 1 ：貪心
---
顯然地直接貪心會有問題，假設優先選擇了某個專案，有可能不選他，去選擇其他的專案，可以賺到更多錢。

想法 2 ： dp
---
考慮某個專案要不要選，似乎沒有簡潔的方法知道其他專案是不是要選。
先考慮選了的情況，假設考慮第一個專案，最大值就是他的價值，
對於後面的專案，最大值是去掉所有前面會衝突的專案，試試看剩下的每個專案選擇會不會更大，再加上這個專案的價值。

如果直接嘗試所有選擇，複雜度會到 $O(2^n)$ ，需要找到一種方法，來減少重複考慮的東西。

可以發現，對於選擇一個專案後，需要考慮的只有會衝突的那些專案要不要選，再往前的不用在現在考慮，因為之前就算出選或不選的最大值了。

因此可以 dp 紀錄選擇每個專案的最大值，接著按照時間順序計算就好。
假設我們從較早的專案開始計算（反過來也行，不影響），結束時間比開始時間更重要，
如果這個專案會衝突，比他晚的結束都會衝突，同理，這個專案不衝突，比他早結束的都不會，
因為有單調性，對結束時間排序，每次再二分搜第一個會衝突的專案，計算在那之後的就可以。

但會有個問題，如果 $n$ 個專案全部重疊，就會需要計算 $n^2$ 項，不能一次計算那麼多項，
要找到只計算一項就能有答案的方法，而專案的開始日期是唯一的，算出某天的最大值就能達到要求， `dp[第i個專案結束日期+1] = max(dp[前一天], dp[第i個專案開始日期] + 第i個專案的價值)`，又因為日期有 $10^9$ 需要做離散化。

注意總金額可能超過 `int` 。

### 範例程式碼

<details>
<summary>C++ 範例</summary>

```cpp
#include <bits/stdc++.h>
#define LL long long
#define N 200005
using namespace std;
LL dp[N << 1] = {0};
int dis[N << 1];
pair<int, pair<int, int>> p[N];
int main() {
	int n, i = 0;
	for(cin >> n; i < n; ++i) {
		cin >> p[i].second.first >> p[i].first >> p[i].second.second;
		//p<結束日期,<開始日期,價值>>
		dis[i << 1] = p[i].first;
		dis[i << 1|1] = p[i].second.first;
	}
	sort(p, p + n);
	sort(dis, dis + (n << 1));
	int len = unique(dis, dis +(n << 1)) -dis, k = 0;
	for(i = 0; i < len; ++i) {
		dp[i + 1] = dp[i];
		for(; k < n && lower_bound(dis, dis + len, p[k].first) -dis < i;) ++k;
		for(; k < n && lower_bound(dis, dis + len, p[k].first) -dis == i; ++k) {
			int start = lower_bound(dis, dis + len, p[k].second.first) -dis,
			val = p[k].second.second;
			dp[i + 1] = max(dp[i + 1], dp[start] + val);
		}
	}
	cout << dp[len];
}
```

</details>