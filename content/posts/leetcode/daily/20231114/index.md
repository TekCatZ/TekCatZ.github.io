---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 14.11.2023 - 1930. Unique Length-3 Palindromic Subsequences"
tags: [ "leetcode", "algorithm" ]
summary: "Giải 1930. Unique Length-3 Palindromic Subsequences"
showToc: true
date: 2023-11-14
---

## Bài toán

Link đến bài toán: [1930. Unique Length-3 Palindromic Subsequences](https://leetcode.com/problems/unique-length-3-palindromic-subsequences)

## Phân tích

Bài này tuy có chữ `Subsequences` nhưng không phải là một bài DP.

Đề bài cho ta là chuỗi Palidrome có độ dài cố định là 3.
Và contrainst đề bài chỉ bao gồm 26 kí tự thường.

Ta sẽ lưu lại vị trí đầu và cuối của mỗi kí tự trong chuỗi. 
Sau đó, ta đếm số kí tự unique giữa mỗi cặp đầu cuối, cũng là số lượng chuỗi
Palindrome độ dài bằng 3 được tạo ra từ kí tự đầu và cuối.

## Giải thuật

1. Khởi tạo 2 mảng `left` và `right` với độ dài 26, mỗi phần tử có giá trị là -1.
2. Duyệt qua chuỗi, lưu lại vị trí đầu và cuối của mỗi kí tự trong chuỗi.
   1. Duyệt qua mảng `left` và `right`, nếu có cặp đầu cuối khác -1, ta sẽ bỏ qua.
       Ngược lại, ta đếm số kí tự unique giữa 2 vị trí đầu cuối, lưu vào một map.
       Sau đó, ta cộng số lượng kí tự unique này vào kết quả.
3. Trả về kết quả.

```go
func countPalindromicSubsequence(s string) int {
	left := make([]int, 26)
	right := make([]int, 26)
	for i := 0; i < 26; i++ {
		left[i] = -1
		right[i] = -1
	}

	var k int32
	for i, c := range s {
		k = c - 'a'
		if left[k] == -1 {
			left[k] = i
		} else {
			right[k] = i
		}
	}

	var charMap map[int]bool
	res := 0
	for i := 0; i < 26; i++ {
		if left[i] == -1 || right[i] == -1 {
			continue
		}

		charMap = make(map[int]bool)
		for j := left[i] + 1; j < right[i]; j++ {
			charMap[int(s[j])] = true
		}

		res += len(charMap)
	}

	return res
}
```