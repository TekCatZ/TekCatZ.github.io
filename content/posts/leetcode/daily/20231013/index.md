---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 13.10.2023 - 746. Min Cost Climbing Stairs"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 746. Min Cost Climbing Stairs"
showToc: true
date: 2023-10-13
---

## Bài toán

Link đến bài toán: [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs)

## Phân tích

Ta có thể ***dễ dàng*** lập công thức đệ quy để giải bài toán này:

```
f(n) = min(f(n - 1), f(n - 2)) + cost[n]
```

Tuy nhiên, sao phải dùng đệ quy khi ta có thể dùng ***dynamic programming***? (=^OwO^=)

Ta có thể dùng một mảng `dp` để lưu lại kết quả của các bài toán con đã được giải.
Với `dp[i]` là chi phí nhỏ nhất để đi đến bậc thang thứ `i`.

```go
dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i]
```

Độ phức tạp của giải thuật là `O(n)`, và độ phức tạp không gian là `O(n)`.

Nếu như quan sát kĩ hơn, ta thấy rằng ta chỉ cần lưu lại hai giá trị `dp[i - 1]` và `dp[i - 2]` thôi.
Lúc này, độ phức tạp không gian sẽ là `O(1)`.

## Giải thuật

### Đệ quy

    1. Nếu `n == 0` hoặc `n == 1` thì `f(n) = cost[n]`.
    2. Nếu `n > 1` thì `f(n) = min(f(n - 1), f(n - 2)) + cost[n]`.

### Dynamic programming

    1. Khởi tạo mảng `dp` với `dp[0] = cost[0]` và `dp[1] = cost[1]`.
    2. Với `i` từ `2` đến `len(cost) - 1`:
        1. `dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i]`.
    3. Trả về `min(dp[len(cost) - 1], dp[len(cost) - 2])`.

### Dynamic programming (không dùng mảng)

    1. Khởi tạo `dp0 = cost[0]` và `dp1 = cost[1]`.
    2. Với `i` từ `2` đến `len(cost) - 1`:
        1. `dp2 = min(dp1, dp0) + cost[i]`.
        2. `dp0 = dp1`.
        3. `dp1 = dp2`.
    3. Trả về `min(dp1, dp0)`.

## Code

### Đệ quy

```go
    func minCostClimbingStairs(cost []int) int {
        n := len(cost)
        if n == 0 || n == 1 {
            return cost[n - 1]
        }
        
        return min(minCostClimbingStairs(cost[:n - 1]), minCostClimbingStairs(cost[:n - 2])) + cost[n - 1]
    }

```

### Dynamic programming

```go
    func minCostClimbingStairs(cost []int) int {
        n := len(cost)
        dp := make([]int, n)
        dp[0] = cost[0]
        dp[1] = cost[1]
        
        for i := 2; i < n; i++ {
            dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i]
        }
        
        return min(dp[n - 1], dp[n - 2])
    }
```

### Dynamic programming (không dùng mảng)

```go
    func minCostClimbingStairs(cost []int) int {
        n := len(cost)
        dp0 := cost[0]
        dp1 := cost[1]
        
        for i := 2; i < n; i++ {
            dp2 := min(dp1, dp0) + cost[i]
            dp0 = dp1
            dp1 = dp2
        }
        
        return min(dp1, dp0)
    }
```