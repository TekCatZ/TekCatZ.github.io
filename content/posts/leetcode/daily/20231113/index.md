---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 13.11.2023 - 2785. Sort Vowels in a String"
tags: [ "leetcode", "algorithm" ]
summary: "Giải 2785. Sort Vowels in a String"
showToc: true
date: 2023-11-13
---

## Bài toán

Link đến bài toán: [2785. Sort Vowels in a String](https://leetcode.com/problems/sort-vowels-in-a-string/)

## Phân tích

Bài này khá dễ, các kí tự là xác đinh trước và cố định, 
vậy ta có thể dùng `counting sort` để giải quyết bài toán này.

## Giải thuật

1. Khởi tạo mảng `resByte ` để chứa kết quả.
2. Khởi tạo mảng `vowelsIndex`  để chứa index của các kí tự nguyên âm trong chuỗi đầu vào.
3. Khởi tạo mảng `vowelsCount` để đếm số lượng các kí tự nguyên âm trong chuỗi đầu vào.
4. Duyệt qua chuỗi đầu vào, nếu kí tự hiện tại là nguyên âm thì thêm vào mảng `vowelsIndex` và tăng `vowelsCount` tương ứng.
5. Khởi tạo biến `idx` để đánh dấu vị trí hiện tại trong mảng `vowelsIndex`.
6. Duyệt qua mảng `vowelsCount`, với mỗi phần tử `count` trong mảng `vowelsCount`:
    - Duyệt từ `0` đến `count`:
        - Thêm kí tự nguyên âm tại vị trí `idx` vào mảng `resByte`.
        - Tăng `idx` lên `1`.
7. Return chuỗi kết quả.

```go
const vowels = "AEIOUaeiou"

func sortVowels(s string) string {
	resByte := []byte(s)
	vowelsIndex := make([]int, 0, len(s))
	vowelCount := make([]int, int('u'-'A')+1)

	for idx, c := range resByte {
		if strings.Contains(vowels, string(c)) {
			vowelsIndex = append(vowelsIndex, idx)
			vowelCount[c-'A']++
		}
	}

	resIdx := 0
	for k, v := range vowelCount {
		for i := 0; i < v; i++ {
			resByte[vowelsIndex[resIdx]] = byte(k + 'A')
			resIdx++
		}
	}

	return string(resByte)
}
```