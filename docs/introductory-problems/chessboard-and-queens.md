---
sidebar_position: 17
---

Chessboard and Queens
===

題目
---
在 $8 \times 8$ 的棋盤中放入 $8$ 個皇后，並且滿足任兩個皇后之間不會互相攻擊。棋盤中有些格子 $*$ 是不能使用的，只有 $\cdot$ 可以放棋子，請問有幾種擺法?

### 輸入
總共有 $8$ 行，每行有 $8$ 個字元

### 輸出
擺法的數量

範例測資
---

```
Input : 
........
........
..*.....
........
........
.....**.
...*....
........

Output : 
65
```

想法
---
> [八皇后問題](https://zh.wikipedia.org/zh-tw/%E5%85%AB%E7%9A%87%E5%90%8E%E9%97%AE%E9%A2%98)是一個很經典的問題，相信有很多人有看過或聽說過著個問題，而 *Chessboard and Queens* 則是八皇后問題的變形

### 遞迴 + 剪枝求解

首先，枚舉所有擺法總共會有 $8^8$ 種方法，證明 : 由左往右每一列都有 $8$ 種可能性，且同一列只能擺一個棋子。但是 $8^8$ 跑太久了，要怎麼減少枚舉的次數呢 ?

我們會觀察到一個性質，當枚舉到下圖的樣子後，繼續枚舉後面幾列是沒有意義的，因為在前兩列就不符合條件了，透過這個性質，可以大量的減少枚舉的次數。

![image](https://hackmd.io/_uploads/rJaE5q3xC.png)

不符合條件的情況有三種 : 
一、在同一行有另一個皇后
二、在對角線有另一個皇后
三、當前的格子是題目要求不能放棋子的格子
因此當我們遇到這三種狀況的時候可以回到前一列繼續枚舉下一種狀況。

<details>
<summary>C++ 範例 </summary>
```cpp
#include <bits/stdc++.h>
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
using namespace std;

char board[8][8];
bool check[8][8];
int ans = 0;

bool ok(int x, int y) {
    for(int i = 0; i < 8; i++) {
        for(int j = 0; j < 8; j++) {
            if(check[i][j] == true) {
                if(abs(x - i) == abs(y - j)) {
                    return false;
                }
                if(x == i || y == j) {
                    return false;
                }
            }
        }
    }
    return true;
}

void solve(int queen) {
    if(queen == 8) {
        ans++;
        return;
    }
    for(int i = 0; i < 8; i++) {
        if(board[queen][i] == '*' || ok(queen, i) == false) {
            continue;
        }
        check[queen][i] = true;
        solve(queen + 1);
        check[queen][i] = false;
    }
}

int main() {
    IO;
    for(int i = 0; i < 8; i++) {
        for(int j = 0; j < 8; j++) {
            cin >> board[i][j];
        }
    }
    solve(0);
    cout << ans;
}
```
</details>