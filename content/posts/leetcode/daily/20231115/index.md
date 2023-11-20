---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 15.11.2023 - 1846. Maximum Element After Decreasing and Rearranging"
tags: [ "leetcode", "algorithm" ]
summary: "Giải 1846. Maximum Element After Decreasing and Rearranging"
showToc: true
date: 2023-11-15
---

## Bài toán

Link đến bài toán: [1846. Maximum Element After Decreasing and Rearranging](https://leetcode.com/problems/maximum-element-after-decreasing-and-rearranging/)

## Phân tích

Diễn giải lại đề bài, thì giả định ta có một mảng `arr` gồm `n` phần tử, và ta có thể thực hiện 2 thao tác sau:
-  Thao tác 1: giảm giá trị của một phần tử trong mảng đi 1 đơn vị.
- Thao tác 2: sắp xếp lại mảng theo thứ tự tăng dần.

Yêu cầu đặt ra là, ta cần tìm giá trị lớn nhất có thể đạt được của phần tử cuối cùng trong mảng sau khi thực hiện 2 thao tác trên.

Nếu chạy thử với các ví dụ, mình có thể nhận ra quy luật rằng,
khi sắp xếp mảng theo thứ tự tăng dần, thì phần tử đầu tiên là `1`,
và với mỗi phần tử `a[i]`, ta có thể đặt giá trị của nó là `min(a[i], a[i-1] + 1)`.
Vậy kết quả cần tìm sẽ là `a[n-1]`.

Nếu nhìn kĩ hơn, ta nhận thấy rằng ta chỉ cần một biến `max`
để lưu giá trị lớn nhất hiện tại nếu ta cập nhập theo công thức trên,
cứ mỗi giá trị mà `max < a[i]`, ta cộng `max` lên 1.

## Giải thuật

1. Sắp xếp mảng theo thứ tự tăng dần.
2. Khởi tạo `max = 1`.
3. Duyệt mảng từ `1` đến `n-1`, nếu `max < a[i]`, `max++`.
4. Trả về `max`.

```go
func maximumElementAfterDecrementingAndRearranging(arr []int) int {
    sort.Ints(arr)
    max := 1
    for i := 1; i < len(arr); i++ {
        if max < arr[i] {
            max++
        }
    }
    return max
}
```