---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 07.11.2023 - 1921. Eliminate Maximum Number of Monsters"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1921. Eliminate Maximum Number of Monsters"
showToc: true
date: 2023-11-07
---

## Bài toán

Link đến bài toán: [1921. Eliminate Maximum Number of Monsters](https://leetcode.com/problems/eliminate-maximum-number-of-monsters/)

## Phân tích

Áp dụng công thức vật lý thời gian bằng quãng đường chia vận tốc, mình suy ra công thức 
`t[t] = ceil(dist[i] / speed[i])`
Với
`t[t]`: thời gian quái thứ `i` đến nơi.

Sau đó, mình sắp xếp lại mảng `t[t]` theo thứ tự tăng dần,
và bởi vì ban đầu súng đã được nạp sẵn, nên ta có thể duyệt từ `i = 1` (thời điểm phút thứ 1).
Sau đó, ta đếm xem tại thời điểm `i` phút thì `t[i]` có lớn hơn `i` hay không, nếu có thì ta tăng biến đếm lên 1.
Nếu không thì ta dừng vòng lặp và trả về biến đếm.

## Giải thuật

```go
func eliminateMaximum(dist []int, speed []int) int {
	n := len(dist)
	timeToReach := make([]int, n)
	for i, v := range dist {
		timeToReach[i] = v / speed[i]
		if v%speed[i] != 0 {
			timeToReach[i]++
		}
	}

	sort.Ints(timeToReach)
	res := 1
	for i := 1; i < n; i++ {
		if timeToReach[i] > i {
			res++
		} else {
			break
		}
	}

	return res
}
```