---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 20.11.2023 - 2391. Minimum Amount of Time to Collect Garbage"
tags: [ "leetcode", "algorithm" ]
summary: "Giải 2391. Minimum Amount of Time to Collect Garbage"
showToc: true
date: 2023-11-20
---

## Bài toán

Link đến bài toán: [2391. Minimum Amount of Time to Collect Garbage](https://leetcode.com/problems/minimum-amount-of-time-to-collect-garbage/)

## Phân tích

Đề bài yêu cầu đơn giản là tính tổng thời gian để thu gom rác của các xe rác.
Nếu đọc đề bài, chỉ có một đường đi duy nhất, nên ta chỉ cần tính thời gian đi từ đầu đến cuối của mỗi xe rồi cộng lại là xong.
Thời gian để 1 xe đi thu hết rác là tổng thời gian thu rác tại mỗi điễm và thời gian di chuyển, vậy chỉ khi các điểm sau có rác ta mới cộng thời gian di chuyển.


## Giải thuật

```go
func garbageCollection(garbage []string, travel []int) int {
	n := len(garbage)
	return calculateTimeByType("M", garbage, travel, n) +
		calculateTimeByType("P", garbage, travel, n) +
		calculateTimeByType("G", garbage, travel, n)
}

func calculateTimeByType(t string, garbage []string, travel []int, n int) int {
	total := 0
	time := 0
	collectTime := 0
	for i, house := range garbage {
		collectTime = strings.Count(house, t)
		if collectTime > 0 {
			total += time + collectTime
			time = 0
		}
		if i+1 < n {
			time += travel[i]
		}
	}

	return total
}

```