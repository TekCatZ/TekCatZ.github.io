---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 21.10.2023 - 1425. Constrained Subsequence Sum"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1425. Constrained Subsequence Sum"
showToc: true
date: 2023-10-21
---

## Bài toán

Link đến bài toán: [1425. Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/)

## Phân tích

Bài toán yêu cầu ta tìm một dãy con của mảng `nums` sao cho tổng của dãy con này là lớn nhất,
và mỗi phần tử trong dãy con này phải cách nhau tối đa `k` phần tử.

Cách đơn giản nhất để giải bài toán này là dùng `dynamic programming`
với `dp[i]` là tổng lớn nhất của dãy con kết thúc tại `i` và thỏa mãn yêu cầu của bài toán.

```
f(i) = max(f(i-1), f(i-2), ..., f(i-k)) + nums[i]
```

Tuy nhiên, với cách giải này, ta sẽ có độ phức tạp là `O(n*k)`, chắc chắn sẽ bị `TLE`.

Ta sẽ tối ưu giải thuật bằng cách sử dụng một `max heap` để lưu các giá trị `f(i-1), f(i-2), ..., f(i-k)`.
Hoặc sử dụng `monotonic queue` để lưu các giá trị `f(i-1), f(i-2), ..., f(i-k)` theo thứ tự giảm dần.

## Giải thuật

### Dynamic programming (Sẽ bị TLE)

```go
func constrainedSubsetSum(nums []int, k int) int {
    n := len(nums)
    dp := make([]int, n)
    dp[0] = nums[0]

    for i := 1; i < n; i++ {
        dp[i] = nums[i]
        for j := max(0, i-k); j < i; j++ {
            dp[i] = max(dp[i], dp[j]+nums[i])
        }
    }

    return max(dp...)
}

func max(nums ...int) int {
    ans := nums[0]
    for _, num := range nums {
        if num > ans {
            ans = num
        }
    }
    return ans
}
```

### Max heap

1. Khởi tạo một `max heap` rỗng, mỗi phần tử trong heap sẽ là một cặp `(value, index)`.
2. Push `(nums[0], 0)` vào heap.
3. Lặp từ 1 đến `n-1`:
    1. Lặp chừng nào `heap.top().index < i-k`:
        1. `pop` `heap`.
    2. Đặt `cur = max(0, h.Top()[0]) + nums[i]`. (Nếu `h.Top()[0] < 0` thì ta sẽ không chọn subsequence phía trước)
    3. `push` `(cur, i)` vào `heap`.
    4. Gán `res = max(ans, cur)`.
4. Trả về `res`.

```go
type MaxHeap struct {
	data [][2]int
}

func (h *MaxHeap) Len() int {
	return len(h.data)
}

func (h *MaxHeap) Less(i, j int) bool {
	return h.data[i][0] > h.data[j][0]
}

func (h *MaxHeap) Swap(i, j int) {
	h.data[i], h.data[j] = h.data[j], h.data[i]
}

func (h *MaxHeap) Push(x interface{}) {
	h.data = append(h.data, x.([2]int))
}

func (h *MaxHeap) Pop() interface{} {
	n := len(h.data)
	x := h.data[n-1]
	h.data = h.data[:n-1]
	return x
}

func (h *MaxHeap) Top() [2]int {
	return h.data[0]
}

func constrainedSubsetSum(nums []int, k int) int {
	n := len(nums)
	h := &MaxHeap{data: make([][2]int, 0)}
	heap.Init(h)

	h.Push([2]int{nums[0], 0})
	res := nums[0]
	for i := 1; i < n; i++ {
		for h.Len() > 0 && i-h.Top()[1] > k {
			heap.Pop(h)
		}

		cur := max(0, h.Top()[0]) + nums[i]
		res = max(res, cur)
		heap.Push(h, [2]int{cur, i})
	}
	return res
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

Độ phức tạp: `O(n*log(n))`

### Monotonic queue

Cách giải này tương tự với cách giải sử dụng `max heap`, nhưng thay vì dùng `max heap`, ta sẽ dùng `monotonic queue`.
Độc giả tự cài đặt nhé.