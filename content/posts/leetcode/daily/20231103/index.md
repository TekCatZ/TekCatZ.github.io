---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 03.11.2023 - 1441. Build an Array With Stack Operations"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1441. Build an Array With Stack Operations"
showToc: true
date: 2023-11-03
---

## Bài toán

Link đến bài toán: [1441. Build an Array With Stack Operations](https://leetcode.com/problems/build-an-array-with-stack-operations/)

## Phân tích

Bài này khó nhất là hiểu được đề bài.
Để diễn giải một cách đơn giản, thì đề bài cho ta một mảng `target` tăng nghiêm ngặt và một số nguyên `n` đại diện cho 1 mảng [1, 2, 3, ..., n].
Nhiệm vụ của ta là in ra các thao tác `Push` và `Pop` vào một stack từ mảng [1, 2, 3, ..., n] sao cho ta có thể tạo ra được mảng `target` từ stack đó.
Cách giải đơn giản là giữ một biến đếm `i` và duyệt qua mảng `target`, mỗi lần duyệt ta kiểm tra `i` có bằng với phần tử hiện tại của `target` hay không.
Nếu bằng thì ta thực hiện `Push` và tăng `i` lên 1, nếu không thì ta thực hiện `Push` và `Pop` (đại diện cho việc bỏ qua phần tử đó) và tăng `i` lên 1.

## Giải thuật

1. Khởi tạo một mảng `res` rỗng để lưu kết quả.
2. Khởi tạo một biến đếm `i` bằng 1.
3. Duyệt qua mảng `target`:
    1. Nếu `i` bằng với phần tử hiện tại của `target`:
        1. Thực hiện `Push` vào `res`.
        2. Tăng `i` lên 1.
    2. Nếu không:
        1. Thực hiện `Push` và `Pop` vào `res`.
        2. Tăng `i` lên 1.
4. Trả về `res`.

```go
func buildArray(target []int, n int) []string {
    var res []string
    i := 1
    for _, v := range target {
        for i < v {
            res = append(res, "Push")
            res = append(res, "Pop")
            i++
        }
    res = append(res, "Push")
    i++
    }
    return res
}
```