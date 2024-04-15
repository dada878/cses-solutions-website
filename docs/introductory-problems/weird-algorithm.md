Weird Algorithm 
===

題目
---
有個演算法接受一個正整數 $n$，並重複以下步驟直到 $n$ 變成 $1$：
- 如果 $n$ 是偶數，就將它除以 $2$；
- 如果 $n$ 是奇數，就將它乘以 $3$ 再加 $1$。

例如從 $n = 3$ 開始，演算法的過程就是
$ 3 \to 10 \to 5 \to 16 \to 8 \to 4 \to 2 \to 1. $

給你一個正整數 $n$，你的任務是模擬這個演算法。

### 輸入
一個正整數 $n$。（$1 \le n \le 10^6$）

### 輸出
將正整數 $n$ 經過演算法產生的所有數值按照順序輸出成一行。

也就是輸出 $n\; k_1\; k_2\; \cdots\; 1$，其中演算法的過程為 $n \to k_1 \to k_2 \to \cdots \to 1$。

範例測資
---
```
Input :
3

Output :
3 10 5 16 8 4 2 1
```

$n = 3$ 是奇數，所以讓 $n = 3 \times 3 + 1 = 10$；
$n = 10$ 是偶數，所以讓 $n = 10 \div 2 = 5$；
$n = 5$ 是奇數，所以讓 $n = 5 \times 3 + 1 = 16$；
以此類推，這個過程會是

$ 3 \to 10 \to 5 \to 16 \to 8 \to 4 \to 2 \to 1$

想法
---
按照題目實作即可。

> [考拉茲猜想](https://zh.wikipedia.org/zh-tw/考拉茲猜想)聲稱任何正整數經過這個演算法都會結束在 $1$，這個猜想目前還沒有被證明出來。但人們已經檢查過在足夠大（遠比 CSES 測資範圍大）的範圍中這個演算法總是會結束。

### 注意事項
使用 C++ 要注意 `int` 在測資較大時會溢位。

### 範例程式碼

<details>
<summary>C++ 範例</summary>
```cpp
 
    #include <iostream>
    using namespace std;    
    int main() {
        long long a;
        cin >> a;
        cout << a << ' ';
        while (a != 1) {
            if (a % 2 == 1) {
                a = a * 3 + 1;
            } else {
                a /= 2;
            }
            cout << a << ' ';
        }
    }
    ```
</details>

<details>
    <summary>Python 範例</summary>

```python
xs = [int(input())]

while xs[-1] != 1:
    if xs[-1] % 2 == 1:
        xs.append(xs[-1] * 3 + 1)
    else:
        xs.append(xs[-1] // 2)

print(*xs)
```
</details>

<details>
<summary>Haskell 範例</summary>

```haskell
collatz :: Int -> [Int]
collatz 1 = [1]
collatz n | even n    = n : collatz (n `div` 2)
          | otherwise = n : collatz (3 * n + 1)

main :: IO ()
main = interact $ unwords . map show . collatz . read
```
</details>
