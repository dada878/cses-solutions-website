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
總共有 65 種合法的擺法

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

更詳細的內容寫在程式碼註解。

### 條件的判斷

假設格子 ($x_1$, $y_1$) 是要檢查且已經有皇后的格子，且 ($x_2$, $y_2$) 是要判斷是否能放皇后的格子，如果 $x_1 = x_2$，代表同一行有兩個可以互相攻擊的皇后，該條件不合法。對角線則是要從斜率判斷，斜率若為 $1$ 或 $-1$，則代表該條直線的皇后都能互相攻擊，因此只要判斷 &#124;$x_1 - x_2$\| 是否等於 &#124;$y_1 - y_2$\| 即可。

<details>
<summary>C++ 範例 </summary>
```cpp
#include <bits/stdc++.h>
#define IO ios_base::sync_with_stdio(0), cin.tie(0)
using namespace std;

char board[8][8];
bool check[8][8];
int ans = 0;

//檢查之前的皇后是否合法
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

//枚舉
void solve(int queen) {
    // 枚舉完八個皇后了，並且都符合條件
    if(queen == 8) {
        ans++;
        return;
    }
    // 枚舉同一列的 8 個位子
    for(int i = 0; i < 8; i++) {
        // 遇到不合法的條件則跳過
        if(board[queen][i] == '*' || ok(queen, i) == false) {
            continue;
        }
        // 將該位子標記為有棋子
        check[queen][i] = true;
        solve(queen + 1);
        // 枚舉完記得要改回來
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