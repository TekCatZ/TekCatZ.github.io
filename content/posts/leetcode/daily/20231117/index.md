---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 17.11.2023 - 1877. Minimize Maximum Pair Sum in Array"
tags: [ "leetcode", "algorithm" ]
summary: "Giải 1877. Minimize Maximum Pair Sum in Array"
showToc: true
date: 2023-11-17
---

## Bài toán

Link đến bài toán: [1877. Minimize Maximum Pair Sum in Array](https://leetcode.com/problems/minimize-maximum-pair-sum-in-array/)

## Phân tích

Phần `medium` là đọc đề thôi, không có gì phức tạp.
Đề yêu cầu rằng, từ mảng `n` phần tử, tạo ra các cặp,
cộng các cặp này,
tìm giá trị lớn nhất trong các tổng này, sao cho giá trị lớn nhất này là `nhỏ nhất`.

Cách làm thì đơn giản, sắp xếp mảng theo thứ tự tăng dần, sau đó lấy phần tử đầu tiên và cuối cùng cộng lại với nhau, lấy giá trị lớn nhất trong các tổng này, đó là kết quả. 

## Giải thuật

1. Sắp xếp mảng theo thứ tự tăng dần.
2. Khởi tạo `max = 0`.
3. Khởi tạo `i = 0`, `j = len(arr) - 1`.
4. Lặp `i < j`:
    1. `max = max(max, arr[i] + arr[j])`.
    2. `i++`, `j--`.
4. Trả về `max`.

```go
func minPairSum(nums []int) int {
    sort.Ints(nums)
    max := 0
    i, j := 0, len(nums) - 1
    for i < j {
        max = max(max, nums[i] + nums[j])
        i++
        j--
    }
    return max
}
```