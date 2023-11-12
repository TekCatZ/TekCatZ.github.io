---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 08.11.2023 - 2849. Determine if a Cell Is Reachable at a Given Time"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 2849. Determine if a Cell Is Reachable at a Given Time"
showToc: true
date: 2023-11-08
---

## Bài toán

Link đến bài toán: [2849. Determine if a Cell Is Reachable at a Given Time](https://leetcode.com/problems/determine-if-a-cell-is-reachable-at-a-given-time/description/)

## Phân tích

Bài này thực ra rất dễ, chỉ có 1 edge case khá khó chịu là `t == 1`.
Để tìm khoảng cách nhỏ nhất giữa 2 điểm trên một grid, ta dùng công thức Chebyshev distance:
```
d = max(abs(x1 - x2), abs(y1 - y2))
```

Mình có thể nhận thấy rằng, miễn là `t >= d`, ta sẽ luôn có thể đi từ điểm bắt đầu đến điểm kết thúc trong `t` bước.
Tuy nhiên, nếu `x == y`, và `t == 1`, thì ta không thể đi được từ điểm bắt đầu đến điểm kết thúc.

## Giải thuật

```go
func isReachableAtTime(sx int, sy int, fx int, fy int, t int) bool {
if fx == sx && fy == sy && t == 1 {
return false
}

return max(math.Abs(float64(fx-sx)), math.Abs(float64(fy-sy))) <= float64(t)
}

func max(a, b float64) float64 {
if a > b {
return a
}
return b
}
```