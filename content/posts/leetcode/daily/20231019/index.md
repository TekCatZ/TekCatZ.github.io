---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 19.10.2023 - 844. Backspace String Compare"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 844. Backspace String Compare"
showToc: true
date: 2023-10-19
---

## Bài toán

Link đến bài toán: [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare)

## Phân tích

Nếu giải theo constraint time complexity là `O(n)` và space complexity là `O(n)`, thì bài này khá đơn giản.
Ta có thể tạo chuỗi mới từ chuỗi ban đầu rồi so sánh hai chuỗi này với nhau.
Hoặc ta có thể dùng stack.

Tuy nhiên, để giải theo constraint time complexity là `O(n)` và space complexity là `O(1)`
như yêu cầu của follow-up thì sẽ hơi khó hơn một tí.
Ta sẽ duyệt chuỗi từ cuối lên đầu, và dùng một biến `skip` để đếm số lượng ký tự cần bỏ qua.

Nếu gặp ký tự `#`, ta sẽ tăng `skip` lên 1. Nếu gặp ký tự khác `#` và `skip` khác 0,
ta sẽ bỏ qua ký tự đó (tức là giảm index và trừ skip đi 1). 

## Giải thuật

### Giải theo constraint time complexity là `O(n)` và space complexity là `O(1)`

1. Khởi tạo `sIdx` và `tIdx` là index của chuỗi `s` và `t`.
2. Khởi tạo `sSkip` và `tSkip` là số lượng ký tự cần bỏ qua của chuỗi `s` và `t`. Ban đầu, `sSkip` và `tSkip` bằng 0.
3. Lặp chừng nào `sIdx >= 0 || tIdx >= 0`:
    1. Với chuỗi s:
       1. Nếu `s[sIdx] == '#'`, tăng `sSkip` lên 1 và giảm `sIdx` đi 1.
       2. Nếu `sSkip > 0`, giảm `sSkip` đi 1 và giảm `sIdx` đi 1.
       3. Nếu không, thoát khỏi vòng lặp.
    2. Tương tự với chuỗi `t`.
    3. Nếu `sIdx >= 0 && tIdx >= 0 && s[sIdx] != t[tIdx]`, trả về `false`. (Vì hai kí tự tại vị trí này không bằng nhau)
    4. Nếu `(sIdx >= 0) != (tIdx >= 0)`, trả về `false`. (Vì hai chuỗi này không bằng nhau)
    5. Giảm `sIdx` và `tIdx` đi 1.
6. Trả về `true`.

```go
func backspaceCompare(s string, t string) bool {
	sIdx, tIdx := len(s)-1, len(t)-1
	sSkip, tSkip := 0, 0

	for sIdx >= 0 || tIdx >= 0 {
		for sIdx >= 0 {
			if s[sIdx] == '#' {
				sSkip++
				sIdx--
			} else if sSkip > 0 {
				sSkip--
				sIdx--
			} else {
				break
			}
		}

		for tIdx >= 0 {
			if t[tIdx] == '#' {
				tSkip++
				tIdx--
			} else if tSkip > 0 {
				tSkip--
				tIdx--
			} else {
				break
			}
		}

		if sIdx >= 0 && tIdx >= 0 && s[sIdx] != t[tIdx] {
			return false
		}

		if (sIdx >= 0) != (tIdx >= 0) {
			return false
		}

		sIdx--
		tIdx--
	}

	return true
}
```

Cách giải dùng stack hoặc tạo chuỗi mới khá dễ nên độc giả tự cài đặt nhé.