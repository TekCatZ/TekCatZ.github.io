---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 05.11.2023 - 1535. Find the Winner of an Array Game"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1535. Find the Winner of an Array Game"
showToc: true
date: 2023-11-05
---

## Bài toán

Link đến bài toán: [1535. Find the Winner of an Array Game](https://leetcode.com/problems/find-the-winner-of-an-array-game)

## Phân tích

Vẫn như mấy bài trước của tháng 11 này, bài này khó hiểu nhất là phần đọc đề.
Bài này là một bài dạng `simulation`, tức là ta sẽ giả lập lại quá trình thực hiện của bài toán.
Để làm được điều này, mình có thể dùng một `deque` để lưu trữ các giá trị của mảng, và một biến `max` để lưu giá trị lớn nhất của mảng.
Tuy nhiên, nếu để ý kĩ, ta có thể nhận ra ta không cần phải `stimulate` đầy đủ.
Nếu `k < n`, ta `stimulate` như bình thường, nhưng nếu `k >= n`, ta chỉ cần tìm giá trị lớn nhất của mảng là được.

## Giải thuật

1. Đặt `max = arr[0]`
2. Đặt `count = 0` để đếm streak
3. Duyệt mảng từ `arr[1:]`:
    1. Nếu `count == k`:
        1. Trả về `max`
    2. Nếu `v > max`:
        1. Gán `max = v`
        2. Gán `count = 1`
    3. Nếu không:
        1. Tăng `count` lên 1
4. Trả về `max`

```go
func getWinner(arr []int, k int) int {
	max := arr[0]

	count := 0
	for _, v := range arr[1:] {
		if count == k {
			return max
		}

		if v > max {
			max = v
			count = 1
		} else {
			count++
		}
	}

	return max
}
```