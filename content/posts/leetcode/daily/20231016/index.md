---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 16.10.2023 - 119. Pascal's Triangle II"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 119. Pascal's Triangle II"
showToc: true
date: 2023-10-16
---

## Bài toán

Link đến bài toán: [119. Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii)

## Phân tích

Như hình minh họa, ta có thể thấy số ở hàng thứ `i` và cột thứ `j` là tổng của 2 số ở hàng `i-1` và cột `j-1` và `j`.

![Pascal's Triangle](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

Vậy ta có thể dùng công thức sau để tính số ở hàng `i` và cột `j`:

```
f(i, j) = f(i-1, j-1) + f(i-1, j)
```

Trong đó:
- `f(0, x), f(x, 0), f(x, x) = 1`

Ngoài ra, nếu ta đọc về Pascal's Triangle
[trên Wikipedia](https://en.wikipedia.org/wiki/Pascal%27s_triangle), ta thấy rằng 
số ở hàng `i` và cột `j` là tổ hợp chập `j` của `i`: ***C<sub>i</sub><sup>j</sup>***.
Công thức tính tổ hợp chập `j` của `i` như sau:

```
f(i, j) = f(i-1, j-1) * (i - j + 1) / j
```

/ᐠ ̥  ̮  ̥ ᐟ\ฅ

## Giải thuật

### Đệ quy

```go
func getRow(rowIndex int) []int {
    f := func(i, j int) int {
        if i == 0 || j == 0 || i == j {
            return 1
        }
        return f(i - 1, j - 1) * (i - j + 1) / j
    }
    res := make([]int, rowIndex + 1)
    for i := 0; i <= rowIndex; i++ {
        res[i] = f(rowIndex, i)
    }
    return res
}
```

Độ phức tạp: **O(2<sup>rowIndex</sup>).**

### Quy hoạch động

```go
func getRow(rowIndex int) []int {
    res := make([][]int, rowIndex + 1)
    for i := 0; i <= rowIndex; i++ {
        res[i] = make([]int, i + 1)
        for j := 0; j <= i; j++ {
            if j == 0 || j == i {
                res[i][j] = 1
            } else {
                res[i][j] = res[i - 1][j - 1] + res[i - 1][j]
            }
        }
    }
    return res[rowIndex]
}
```

Độ phức tạp: **O(rowIndex<sup>2</sup>).**

### Toán /ᐠ - ˕ -マ Ⳋ

```go
func getRow(rowIndex int) []int {
    res := make([]int, rowIndex + 1)
    res[0] = 1
    for i := 1; i <= rowIndex; i++ {
        res[i] = res[i - 1] * (rowIndex - i + 1) / i
    }
    return res
}
```

Độ phức tạp: **O(rowIndex).**