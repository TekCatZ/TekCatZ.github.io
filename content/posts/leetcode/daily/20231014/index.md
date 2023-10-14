---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 14.10.2023 - 2742. Painting the Walls"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 2742. Painting the Walls"
showToc: true
date: 2023-10-14
---

## Bài toán

Link đến bài toán: [2742. Painting the Walls](https://leetcode.com/problems/painting-the-walls/)

## Phân tích

Đề bài đọc khá khó hiểu, /ᐠﹷ ‸ ﹷ ᐟ\ﾉ
để giải thích cho dễ hiểu thì đề bài yêu cầu như thế này:

- Có 2 mảng `cost` và `time`, cùng độ dài,
  `cost[i]` là chi phí để sơn tường thứ `i`,
  `time[i]` là thời gian để sơn tường thứ `i`.
- `paid painter` sẽ sơn 1 bức tường với chi phí là `cost[i]` và thời gian là `time[i]`.
- `free painter` sẽ sơn 1 bức tường với chi phí là 0, thời gian là 1.
- Mục tiêu là tìm cách sơn tường với chi phí nhỏ nhất.

Nhìn 1 cách đơn giản hóa bài toán, thì khi ta chọn một bức tường,
thực chất ta đang sơn `time[i] + 1` (`free painter` sơn time[i] bức, `paid painter` sơn 1 bức)
bức tường với chi phí là `cost[i]`.

Vậy, thực chất đây là một bài toán knapsack chọn hoặc không chọn.
Ta có thể giải bài toán này bằng đệ quy và tối ưu bằng dynamic programming.

Công thức đệ quy:

```
f(i, remain) = min(f(i + 1, remain), f(i + 1, remain - 1 - time[i]) + cost[i])
```

Trong đó

- `f(i, remain)` là chi phí nhỏ nhất để sơn `remain` bức tường từ `i` đến `len(cost) - 1`
- `f(i + 1, remain)` có thể hiểu là ta không sơn tường thứ `i`
- `f(i + 1, remain - 1 - time[i]) + cost[i]` có thể hiểu là ta sơn tường thứ `i`.
  Chi phí bằng chi phí để sơn tường thứ `i` cộng với chi phí để sơn `remain - 1 - time[i]` bức tường từ `i + 1`
  đến `len(cost) - 1`.

Độ phức tạp là **O(2<sup>n</sup>).**

## Giải thuật

### Đệ quy

1. Base case:
    1. remain <= 0 => ta đã sơn xong tất cả các tường => return 0
    2. i == len(cost) => không thể dùng thêm painter nào nữa => return một số lớn
2. Nếu `remain > 0` và `i < len(cost)`:
    1. Tính minCost nếu ta không sơn tường i: `minCost = f(i + 1, remain)`
    2. Tính minCost nếu ta sơn tường i: `minCost = min(minCost, f(i + 1, remain - 1 - time[i]) + cost[i])`
3. Trả về `minCost` nhỏ nhất.

```go
  func minCost(cost []int, time []int) int {
return f(0, len(cost), 0, cost, time)
}

func f(i, n, remain int, cost, time []int) int {
if remain <= 0 {
return 0
}
if i == n {
return 1e9
}
minCost := f(i + 1, n, remain, cost, time)
minCost = min(minCost, f(i + 1, n, remain - 1 - time[i], cost, time) + cost[i])
return minCost
}
```

Tối ưu: Nếu để ý kĩ, ta sẽ thấy nhiều trường hợp `f(i, remain)` được gọi lại nhiều lần. Ta có thể tối ưu bằng cách
dùng một mảng nhớ lại kết quả của các bài toán con đã được giải. Hoặc có thể dủng quy hoạch động? (=^OwO^=)

### Dynamic programming

1. Khởi tạo mảng `dp` với `dp[i][j]` là chi phí nhỏ nhất để sơn `j` bức tường từ `i` đến `len(cost) - 1`,
   giá trị khởi tạo là `0`.
2. Đặt `dp[len(cost) - 1][0] = 1e9`. (Hoặc 1 số lớn nào đó khác)
3. Với `i` từ `len(cost) - 1` đến `0`:
    1. Với `remain` từ `1` đến `len(cost)`:
        1. Tính minCostNotPaint khi ta không sơn tường `i`: `minCost = dp[i + 1][remain]`.
        2. Tính minCostPaint khi ta sơn tường `i`: `minCost = dp[i + 1][max(0, remain - 1 - time[i])] + cost[i]`.
        3. `dp[i][remain] = min(minCostNotPaint, minCostPaint)`.
4. Trả về `dp[0][len(cost)]`.

```go
func minCost(cost []int, time []int) int {
    n := len(cost)
    dp := make([][]int, n + 1)
    for i := 0; i <= n; i++ {
        dp[i] = make([]int, n + 1)
        dp[i][0] = 1e9
    }
	
    for i := n - 1; i >= 0; i-- {
        for remain := 1; remain <= n; remain++ {
            minCostNotPaint := dp[i + 1][remain]
            minCostPaint := dp[i + 1][max(0, remain - 1 - time[i])] + cost[i]
            dp[i][remain] = min(minCostNotPaint, minCostPaint)
        }
    }
    return dp[0][n]
}
```

Độ phức tạp là **O(n<sup>2</sup>)**. Độ phức tạp không gian là **O(n<sup>2</sup>)**.

Tối ưu: Ta thấy rằng ta chỉ cần lưu lại 2 hàng của mảng `dp` thôi. Lúc này, độ phức tạp không gian sẽ là `O(n)`.
Các bạn tự giải tiếp nhé, bài này được viết vào nửa đêm, tác giả thì đã buồn ngủ và lười viết tiếp. /ᐠ - ˕ -マ Ⳋ