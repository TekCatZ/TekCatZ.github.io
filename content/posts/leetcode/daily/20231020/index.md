---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 20.10.2023 - 341. Flatten Nested List Iterator"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 341. Flatten Nested List Iterator"
showToc: true
date: 2023-10-20
---

## Bài toán

Link đến bài toán: [341. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/)

## Phân tích

Đề bài yeu cầu ta implement một `iterator` cho một list các `NestedInteger`,
mỗi `NestedInteger` có thể là một số nguyên hoặc một list các `NestedInteger` khác.

Có 2 hướng giải quyết bài toán này: "lazy" và "eager".

Đối với hướng "eager", tại thời điểm khởi tạo `iterator`, ta dùng đệ quy để duyệt qua toàn bộ `NestedInteger` và lưu lại kết quả vào một mảng.

Đối với hướng "lazy", ta sẽ xử lý từng `NestedInteger` khi cần thiết (cụ thể là khi hàm `HasNext()` và `Next()` được gọi).

## Giải thuật

### Eager

Kiểu Iterator của ta sẽ chứa một mảng `nums` và một biến `idx` để lưu vị trí hiện tại.

Ở constructor:
1. Với mỗi `NestedInteger` trong `nestedList`, ta sẽ gọi hàm `flatten` để duyệt qua toàn bộ `NestedInteger` và lưu kết quả vào `nums`.

Ở hàm `flatten`:
1. Tạo một mảng tạm `nums` rỗng.
2. Nếu `nestedInteger` là một số nguyên, ta sẽ thêm nó vào `nums`.
3. Nếu `nestedInteger` là một list các `NestedInteger`, ta sẽ gọi đệ quy hàm `flatten` với `nestedInteger` này.
4. Trả về `nums`.

Ở hàm `HasNext()`:
1. Trả về `idx < len(nums)`.

Ở hàm `Next()`:
1. Tăng `idx` lên 1.
2. Trả về `nums[idx-1]`.

Độ phức tạp: `O(n)`
Độ phức tạp không gian: `O(n)`

### Lazy

Kiểu Iterator của ta sẽ chứa một stack `internalStack`.

Ở constructor:
1. Đưa toàn bộ `nestedList` vào `internalStack`.

Ở hàm `unpack()`:
1. Lặp chừng nào `internalStack` không rỗng và `internalStack.top()` không phải số nguyên:
   1. Lấy `top` của `internalStack` ra và gán vào `nestedInteger`.
   2. Với từng `NestedInteger` trong `nestedInteger.getList()`, ta đẩy nó vào đầu stack.

Ở hàm `HasNext()`:
1. Gọi hàm `unpack()`.
2. Trả về `len(internalStack) > 0`.

Ở hàm `Next()`:
1. Gọi hàm `unpack()`.
2. Lấy `top` của `internalStack` ra.
3. `pop` `internalStack`.
4. Trả về `top`.

Độ phức tạp: `O(n)`
Độ phức tạp không gian: Nhỏ hơn `O(n)` do ta không cần lưu toàn bộ `nestedList` vào `internalStack`.

```go
type NestedIterator struct {
	internalStack []*NestedInteger
}

func Constructor(nestedList []*NestedInteger) NestedIterator {
	return NestedIterator{
		internalStack: nestedList,
	}
}

func (this *NestedIterator) Next() int {
	this.unpack()
	top := this.internalStack[0]
	this.internalStack = this.internalStack[1:]
	return top.GetInteger()
}

func (this *NestedIterator) HasNext() bool {
	this.unpack()
	return len(this.internalStack) > 0
}

func (this *NestedIterator) unpack() {
	for len(this.internalStack) > 0 {
		top := this.internalStack[0]
		if top.IsInteger() {
			break
		}
		this.internalStack = this.internalStack[1:]
		list := top.GetList()
		for i := len(list) - 1; i >= 0; i-- {
			this.internalStack = append([]*NestedInteger{list[i]}, this.internalStack...)
		}
	}
}
```
