---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 24.10.2023 - 515. Find Largest Value in Each Tree Row"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 515. Find Largest Value in Each Tree Row"
showToc: true
date: 2023-10-24
---

## Bài toán

Link đến bài toán: [515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row)

## Phân tích

Bài này khá dễ, ta dùng BFS hay DFS đều được.

## Giải thuật

### BFS

1. Khởi tạo một queue rỗng, một mảng rỗng `result` để lưu kết quả.
2. Thêm node gốc vào queue.
3. Khởi tạo `level = 0` 
4. Lặp chừng nào queue không rỗng:
    1. Đặt `levelCount` là độ dài của queue (cũng là số node ở level hiện tại).
    2. Với từng node trong `levelCount` node:
       1. Lấy node ra khỏi queue.
       2. Set lại `result[level] = max(result[max], node.Val)`
       3. Nếu node có con trái, thêm con trái vào queue.
       4. Nếu node có con phải, thêm con phải vào queue.
    3. Tăng `level` lên 1.
5. Trả về `result`.

### DFS

1. Khởi tạo một mảng rỗng `result` để lưu kết quả.
2. Tạo một stack rỗng, chứa pair `(node, level)`.
3. Thêm node gốc vào stack.
4. Lặp chừng nào stack không rỗng:
   1. Lấy node và level ra khỏi stack.
   2. Nếu `result[level]` chưa được set, set `result[level] = node.Val`.
   3. Nếu `result[level]` đã được set, set `result[level] = max(result[level], node.Val)`.
   4. Nếu node có con phải, thêm con phải vào stack với level tăng lên 1.
   5. Nếu node có con trái, thêm con trái vào stack với level tăng lên 1.
5. Trả về `result`.

Cài đặt khá đơn giản, tác giả lười nên mọi người tự code nhé.