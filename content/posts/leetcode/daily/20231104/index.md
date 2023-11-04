---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 04.11.2023 - 1503. Last Moment Before All Ants Fall Out of a Plank"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1503. Last Moment Before All Ants Fall Out of a Plank"
showToc: true
date: 2023-11-03
---

## Bài toán

Link đến bài toán: [1503. Last Moment Before All Ants Fall Out of a Plank](https://leetcode.com/problems/last-moment-before-all-ants-fall-out-of-a-plank)

## Phân tích

Như mấy bài trong tuần này, bài này khó nhất là hiểu được đề bài.
Đề bài có một chi tiết rằng, khi hai con kiến gặp nhau, chúng sẽ đổi hướng đi. Nhưng, nếu ta để ý kĩ thì hai con kiến có đổi hướng hay không thì tổng quãng đường mà chúng đi vẫn là như cũ.
Vậy nên đây là 1 chi tiết đánh lạc hướng, ta bỏ qua chi tiết này thì ta nhận thấy rằng thời gian mà đề bài yêu cầu là thời gian lớn nhất mà một con kiến đi được.


## Giải thuật

1. Khởi tạo một biến `maxDistance` để lưu giá trị lớn nhất của khoảng cách mà một con kiến đi được.
2. Duyệt mảng `right` và tìm khoảng cách lớn nhất mà một con kiến đi được, tức là `n - v` (v là giá trị của phần tử hiện tại).
3. Duyệt mảng `left` và tìm khoảng cách lớn nhất mà một con kiến đi được, tức là `v` (v là giá trị của phần tử hiện tại).
4. Trả về `maxDistance`.

```go
func getLastMoment(n int, left []int, right []int) int {
	maxDistance := 0
	for _, v := range right {
		maxDistance = max(maxDistance, n-v)
	}

	for _, v := range left {
		maxDistance = max(maxDistance, v)
	}

	return maxDistance
}
```