---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 22.10.2023 - 1793. Maximum Score of a Good Subarray"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1793. Maximum Score of a Good Subarray"
showToc: true
date: 2023-10-22
---

## Bài toán

Link đến bài toán: [1793. Maximum Score of a Good Subarray](https://leetcode.com/problems/maximum-score-of-a-good-subarray)

## Phân tích

Bài toán này có thể giải bằng cách duyệt mảng và tìm ra các cặp `(left, right)` sao cho `left <= i <= right`
và `min(nums[left:right+1]) * (right - left + 1)` là lớn nhất.

Để tìm được cặp `(left, right)` như vậy, ta có thể dùng `monotonic stack`, hoặc kĩ thuật `two pointers`.

## Monotonic stack

***Monotonic stack*** là một cấu trúc dữ liệu dạng stack, trong đó đảm bảo rằng các phần tử trong stack luôn được sắp xếp theo một thứ tự nhất định.
Nếu ta thêm một phần tử nghịch thế với thứ tự này, thì ta sẽ `pop` các phần tử trong stack cho đến khi phần tử đó thỏa mãn thứ tự của stack.
Tham khảo thêm tại [đây](https://www.geeksforgeeks.org/introduction-to-monotonic-stack-data-structure-and-algorithm-tutorials).

Vậy làm sao để áp dụng `monotonic stack` vào bài toán này?

Ta sẽ khởi tạo 2 mảng `left` và `right`
với `left[i]` là vị trí đầu tiên bên trái của `i` mà `nums[left[i]] < nums[i]`
và `right[i]` là vị trí đầu tiên bên phải của `i` mà `nums[right[i]] < nums[i]`.
Lúc này, mỗi cặp `(left[i], right[i])` sẽ là một cặp `(left, right)` thỏa mãn yêu cầu của bài toán.

Sau đó, ta sẽ duyệt mảng `nums` từ trái sang phải, và với mỗi phần tử `nums[i]`, nếu `left[i] < k` và `right[i] > k` (thỏa điều kiện k)
thì ta sẽ tính `max(ans, nums[i] * (right[i] - left[i] - 1))`.

Độ phức tạp: `O(n)`

## Two pointers

Ta sẽ khởi tạo 2 con trỏ `left` và `right` tại vị trí `k`.
Nếu `nums[left-1] < nums[right+1]` thì ta sẽ tăng `right` lên 1 đơn vị, ngược lại ta sẽ giảm `left` đi 1 đơn vị.
Với mỗi cặp `(left, right)` ta sẽ tính luôn `max(ans, min(nums[left:right+1]) * (right - left + 1))`.

Độ phức tạp: `O(n)`

## Giải thuật

### Monotonic stack

1. Khởi tạo mảng `left` và `right` với `left[i] = -1` và `right[i] = n`.
2. Khởi tạo một `monotonic stack` rỗng.
3. Lặp `i` từ `0` đến `n`:
    1. Lặp chừng nào `stack` không rỗng và `nums[stack.top()] > nums[i]`:
        1. Lấy `top` của `stack` ra và gán `left[top] = i`.
        2. `pop` `stack`.
    2. `push` `i` vào `stack`.
4. Lặp `i` từ `n-1` đến `0`:
    1. Lặp chừng nào `stack` không rỗng và `nums[stack.top()] > nums[i]`:
        1. Lấy `top` của `stack` ra và gán `right[top] = i`.
        2. `pop` `stack`.
    2. `push` `i` vào `stack`.
5. Khởi tạo `ans` bằng 0.
6. Lặp `i` từ `0` đến `n`:
    1. Nếu `left[i] < k` và `right[i] > k`:
        1. Gán `ans = max(ans, nums[i] * (right[i] - left[i] - 1))`.
7. Trả về `ans`.

### Two pointers

1. Khởi tạo `left` và `right` bằng `k`.
2. Khởi tạo `ans` bằng `nums[k]`.
3. Khởi tạo `minVal` bằng `nums[k]`.
4. Lặp chừng nào `left > 0 || right < n-1`:
    1. Nếu `left == 0` hoặc `nums[left-1] < nums[right+1]`:
        1. Tăng `right` lên 1 đơn vị.
        2. Gán `minVal = min(minVal, nums[right])`.
    2. Ngược lại:
        1. Giảm `left` đi 1 đơn vị.
        2. Gán `minVal = min(minVal, nums[left])`.
    3. Gán `ans = max(ans, minVal * (right - left + 1))`.
5. Trả về `ans`.

```go
func maximumScore(nums []int, k int) int {
	n := len(nums)
	i, j := k, k
	res := nums[k]
	minVal := nums[k]
	for i > 0 || j < n-1 {
		if i == 0 {
			j++
		} else if j == n-1 {
			i--
		} else if nums[i-1] < nums[j+1] {
			j++
		} else {
			i--
		}
		minVal = min(minVal, min(nums[i], nums[j]))
		res = max(res, minVal*(j-i+1))
	}

	return res
}

func min(a int, b int) int {
	if a < b {
		return a
	}
	return b
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```