---
sidebar_position: 15
---

Increasing Subsequence
===

## 題目
給一個陣列，求最長嚴格遞增子序列的長度。

### 輸入
第一行一個數 $n$ 代表陣列的大小（ $1\leq n \leq 2\times10^5$ ）。
第二行 $n$ 個數，代表陣列中每項的值 $x_i$（ $1\leq x_i\leq10^9$ ）。

### 輸出
共一數，代表最長嚴格遞增子序列的長度。

範例測資
---
```
Input:
8
7 3 5 3 6 2 9 8

Output:
4
```

想法 1：貪心（但沒有試過所有可能）
---
枚舉起點，盡可能地放數字，看到能加進來就放，會發現 `1 5 2 3 4` 這筆測資有錯。
顯然，這個貪心沒有辦法證明他是對的，也構造了一筆反例，所以不能這樣解。

想法 2：在候選的所有可能中選比較好的
---
換個方式，考慮前面所有排法，加上現在的數。
而如果不同排法的結尾是同一個數，那挑最長的就好，
（結尾相同時，能加的數相同，短的一定不是答案），
因此只要記錄結尾是某個數時，最長可以多長，其中最大的就是答案。

如果用陣列儲存，會因為值域有 $10^9$ 而開不了那麼大的陣列，
一種做法是離散化，但是每次插數字要考慮 $n$ 種不同的結尾，因此複雜度是 $O(n^2)$ 。

比起用離散化，另一種方法則是不存結尾是甚麼值，改存是陣列的哪項，有相同的效果還更方便，考慮第 $i$ 個數時，前面有 $i-1$ 個結尾要考慮，複雜度是 $O(\sum_{i=1}^n(i-1)=\frac{n(n-1)}2)$ ，常數變為一半，對於總數有限的東西很適合用這種方式處理，不過在這題仍然太慢。

想法 3：只存最重要的資料
---
重新看一下我們需要考慮甚麼，需要紀錄甚麼，
`7 3 5 3 6 2 9 8` 以範例測資來說，到最後待選的解有以下這些。
```
7,9
7,8
3,5,6,9
3,5,6,8
5,6,9
5,6,8
3,6,9
3,6,8
6,9
6,8
2,9
2,8
9
8
```
不難發現，有些段落是完全重複的，這些東西可以想辦法合併在一起處理。
而對於那些一樣長但結尾不同的解，顯然選擇更小的結尾，在未來插入其他數字時會有更多選擇，必定會是更好的，因此我們可以選擇覆蓋掉那些只有結尾更小的解，此時，便有一部份重複的東西被我們合併了。

帶著這個想法，我們完整的模擬一次待選的解會有哪些。
```
//插入7
7

//插入3
3  //此時長度同為1 用3覆蓋7永遠更好

//插入5
3,5  //此時我們並不用儲存5這個解 因為他一定比3,5更差
3

//插入3
3,5  //此時一樣不用儲存3這個解 因為長度同為1的情況下 3,5最前面的3有相同的效果
3

//插入6
3,5,6  //情況同插入5
3,5
3

//插入2
3,5,6
3,5
2      //2可以取代3

//插入9
3,5,6,9
3,5,6
3,5
2
//沒有2,9 因為3,5比他更好

//插入8
3,5,6,8
3,5,6
3,5
2
```
這樣我們就透過貪心刪掉許多解了，但顯然的，一筆完全遞增的測資會造成 $n$ 個解，複雜度會和想法 2 一樣。
而對於每個長度，都會有個現在最好的解，只要能記錄每個長度下最好的解就夠了。
每次插入數字，只需要看看在各個長度下加進去會不會變好，但是每次考慮 $n$ 個長度並沒有改善時間複雜度，只是讓空間降到了 $O(n)$ 。
但是我們還沒用上遞增數列的性質，可以推論一下每次插入能不能少考慮一點東西：
1. 在最後面插入數字時，數字一定會比前面的都大。
2. 修改長度較短的解時，數字一定會比原本的小，因此比後面的數字都小。
3. 修改的時候要保證解是遞增的，因此前面的數字要比他小。
4. 不會有長度較小的解結尾比長度較大的解結尾更小的狀況，否則那個解的結尾可以改掉

有了這四點我們會發現，修改或插入數字時，要盡可能地往前放，
並且每個長度現在最好的結尾也會是嚴格遞增的。
因此可以二分搜這個數字要被插入到哪個長度，最終這個陣列的長度就是答案了。

實作時要小心，為了不浪費擴充 `vector` 的時間，一開始就預先配了 $n$ 格的大小，
但還未用到的格子也不能小於前面，否則內建二分搜會有問題。

<details>
<summary>C++ 範例</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, x, ans = 0;
    cin >> n;
    vector<int> v(n, 1 << 30);
    for(; n--;) {
        cin >> x;
        auto it = lower_bound(v.begin(), v.end(), x);
        if(*it == 1 << 30) ++ans;
        *it = x;
    }
    cout << ans;
}
```

</details>

想法 4：區間問題
---
想法 3 是一個很經典的解，但不算特別好想，因此給出另一個我看完後的直覺解。
假設已經插入了 $k$ 個數字，所有結尾是 $x$ 的解最長長度是 $ans[x]$ ，那麼假設下一個要插入的數字是 $i$ ， $ans[i]=max(ans[0],ans[1],\cdots,ans[i-1])$ ，顯然 $ans[0]=0$ 。
因此問題被我們轉化為找區間最大值，可以用 BIT 或線段樹，但因為值域的問題，要用動態開點或離散化。
過程中順便記一下現在所有解的最大值是甚麼。

由於用到了離散化或動態開點，因此常數會比想法 3 略大一點。

### 範例程式碼 1 ：動態開點線段樹（左閉右開）
```cpp
#include <bits/stdc++.h>
using namespace std;
#define N 1000000005
typedef struct Seg {
    int val, l, r;
    Seg *lnode, *rnode;
} Seg;
Seg *root;
int query(Seg *seg, int x) {//回傳[0~x)中的最大值
    if(!seg) return 0;
    if(seg -> r == x) return seg -> val;
    int mid = seg -> l + seg -> r >> 1;
    if(mid < x) return max(query(seg -> lnode, mid), query(seg -> rnode, x));
    return query(seg -> lnode, x);
}
int update(Seg *seg, int pos) {
    //插入一個pos 因此結尾是pos的最大可能要更新為[0~pos)現在的最大值+1
    if(seg -> l == pos && seg -> r - 1 == pos) return seg -> val = 1 + query(root, pos);
    int mid = seg -> l + seg -> r >> 1;
    if(pos < mid) {
        if(!seg -> lnode) {
        	seg -> lnode = (Seg*)malloc(sizeof(Seg));
        	*seg -> lnode = (Seg){0, seg -> l, mid, NULL, NULL};
        }
        return seg -> val = max(seg -> val, update(seg -> lnode, pos));
    }
    else{
        if(!seg -> rnode) {
        	seg -> rnode = (Seg*)malloc(sizeof(Seg));
        	*seg -> rnode = (Seg){0, mid, seg -> r, NULL, NULL};
        }
        return seg -> val = max(seg -> val, update(seg -> rnode, pos));
    }
}
int main() {
    int n, x, ans = 0;
    cin >> n;
    root = (Seg*)malloc(sizeof(Seg));
    *root = (Seg){0, 0, N, NULL, NULL};
    for(; n--;) {
        cin >> x;
        update(root, x);
        x = query(root, x + 1);//最終結果要包含x這一項 因此查詢時要加1
        if(x > ans) ans = x;
    }
    cout << ans;
}
```
### 範例程式碼 2 ：離散化 + BIT
注意 BIT 是 1-base 的，因此離散化後要從 $1$ 開始。
```cpp
#include <bits/stdc++.h>
using namespace std;
int a[200005],dis[200005],bit[200005];
#define lowbit(x) (x & -x)
int query(int x){//回傳[0,x)中的最大值
	int ans = 0;
	for(--x; x > 0; x -= lowbit(x)) if(bit[x] > ans) ans = bit[x];
	return ans;
}
void update(int x, int n){
	//插入一個x 因此結尾是x的最大可能要更新為[0~x)現在的最大值+1
	for(int ans = query(x) + 1; x <= n; x += lowbit(x)) bit[x] = max(ans, bit[x]);
}
int main() {
    int n, i = 0, x, ans = 0;
    for(cin >> n; i < n; ++i) cin >> a[i], dis[i] = a[i];
    sort(dis, dis + n);
    int len = unique(dis, dis + n) -dis;
    for(i = 0; i < n; ++i) {
    	a[i] = lower_bound(dis, dis + len, a[i]) -dis + 1;
    	update(a[i], len + 1);//查詢時需要多往後查一格 因此更新時要多1
    	x = query(a[i] + 1);//最終結果要包含a[i]這一項 因此查詢時要加1
    	if(x > ans) ans = x;
    }
    cout << ans;
}
```