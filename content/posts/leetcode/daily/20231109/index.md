---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 09.11.2023 - 1759. Count Number of Homogenous Substrings"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1759. Count Number of Homogenous Substrings"
showToc: true
date: 2023-11-09
---

## Bài toán

Link đến bài toán: [1759. Count Number of Homogenous Substrings](https://leetcode.com/problems/count-number-of-homogenous-substrings/description/)

## Phân tích

Bài này đơn giản, ta có thể dùng two pointer để giải quyết, hoặc nhanh hơn thì có thể dùng công thức cấp số cộng.

```
n * (n + 1) / 2
```

Ta dùng một biến đếm để đếm số lượng kí tự liên tiếp, nếu gặp kí tự khác thì tính số lượng substring có thể tạo ra từ số lượng kí tự liên tiếp đó, sau đó reset biến đếm.

## Giải thuật

```go
const MOD = 1e9 + 7

func countHomogenous(s string) int {
	res := 0
	streak := 0
	prevChar := int32(0)
	for _, c := range s {
		if prevChar != c {
			res = res + int(arithmeticProgression(streak)%MOD)
			streak = 1
			prevChar = c
		} else {
			streak += 1
		}
	}

	res = res + int(arithmeticProgression(streak)%MOD)

	return res
}

func arithmeticProgression(n int) int64 {
	return int64(0.5 * float64(n) * float64(1+n))
}
```