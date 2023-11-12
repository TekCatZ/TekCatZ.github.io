---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 26.10.2023 - 823. Binary Trees With Factors"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 823. Binary Trees With Factors"
showToc: true
date: 2023-10-26
---

## Bài toán

Link đến bài toán: [823. Binary Trees With Factors](https://leetcode.com/problems/binary-trees-with-factors/)

## Phân tích

Ta có tính chất, với một node có hai con `a` và `b`, thì số cây có thể tạo ra từ node đó là `a * b + 1` (+ cây chỉ có node gốc).
Vậy, ta có công thức đệ quy:

```
f(i) = f(j) * f(i / j) + 1
```

Ta có thể sử dụng một map để lưu kết quả của `f(i / j)` để tránh tính lại.

## Giải thuật

1. Sắp xếp mảng `arr` tăng dần.
2. Tính `f(i)` với `i` từ `0` đến `n - 1` với công thức trên.
3. Tổng hợp kết quả.

### Khử đệ quy

```go
import "sort"

const MOD = 1e9 + 7

func numFactoredBinaryTrees(arr []int) int {
	n := len(arr)
	sort.Ints(arr)

	m := make(map[int]int, n)
	for i := range arr {
		m[arr[i]] = 0
	}

	var res int64 = 0
	var tmp int64
	for i := 0; i < n; i++ {
		for j := i - 1; j >= 0; j-- {
			if arr[i]%arr[j] == 0 {
				quotient := arr[i] / arr[j]
				if v, ok := m[quotient]; ok {
					tmp = (int64(m[arr[j]]) * int64(v)) % int64(MOD)
					m[arr[i]] += int(tmp)
					m[arr[i]] %= MOD
				}
			}
		}
		m[arr[i]] += 1

		res %= MOD
		res += int64(m[arr[i]]) % MOD
	}

	return int(res % MOD)
}
```
/ᐠ - ˕ -マ Ⳋ