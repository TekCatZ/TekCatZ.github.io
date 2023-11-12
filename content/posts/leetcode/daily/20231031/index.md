---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 31.10.2023 - 2433. Find The Original Array of Prefix Xor"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 2433. Find The Original Array of Prefix Xor"
showToc: true
date: 2023-10-31
---

## Bài toán

Link đến bài toán: [2433. Find The Original Array of Prefix Xor](https://leetcode.com/problems/find-the-original-array-of-prefix-xor/description/)

## Phân tích

Bài này không khó đâu. Kiến thức cần biết để làm bài này là:
```
a ^ b = c
=> a ^ c = b
=> b ^ c = a
```

Theo dề bài:
```
pref[i] = pref[i-1] ^ arr[i]
=> arr[i] = pref[i] ^ pref[i-1]
```

Vậy ta loop qua `pref` và tính `arr[i]` rồi thêm vào mảng `arr` là xong.

## Giải thuật

```go
func findArray(pref []int) []int {
	n := len(pref)
	arr := make([]int, n)
	arr[0] = pref[0]
	for i := 1; i < n; i++ {
		arr[i] = pref[i] ^ pref[i-1]
	}

	return arr
}
```

Ta có thể tối ưu bộ nhớ bằng cách dùng `pref` để lưu kết quả luôn bằng cách loop ngược từ `n-1` về `0`. Các bạn thử tự cài đặt xem như thế nào nhé.  /ᐠ - ˕ -マ Ⳋ
