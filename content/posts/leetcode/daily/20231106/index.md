---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 06.11.2023 - 1845. Seat Reservation Manager"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1845. Seat Reservation Manager"
showToc: true
date: 2023-11-06
---

## Bài toán

Link đến bài toán: [1535. Find the Winner of an Array Game](https://leetcode.com/problems/seat-reservation-manager)

## Phân tích

Bài hôm nay khá dễ.

Ta cần tìm một cấu trúc dữ liệu cho phép ta truy cập vào phần tử nhỏ nhất với độ phức tạp là `O(1)`.

=> có thể dùng một `min heap` để giải quyết bài này.

Ta có thể `push` tất cả các số từ 1 đến `n` vào `min heap`, khi `reserve` thì ta `pop` ra một phần tử nhỏ nhất,
khi `unreserve` thì ta `push` lại phần tử đó vào `min heap`.

Có thể optimize hơn bằng cách dùng một `idx` để lưu giá trị nhỏ nhất hiện tại,
khi `reserve` thì ta `pop` ra phần tử nhỏ nhất, hoặc `idx` nếu `min heap` rỗng.

## Giải thuật

```go
type Heap []int

func (h Heap) Len() int           { return len(h) }
func (h Heap) Less(i, j int) bool { return h[i] < h[j] }
func (h Heap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *Heap) Push(x interface{}) {
*h = append(*h, x.(int))
}

func (h *Heap) Pop() interface{} {
old := *h
n := len(old)
x := old[n-1]
*h = old[0 : n-1]
return x
}


type SeatManager struct {
h Heap
idx int
}

func Constructor(n int) SeatManager {
h := Heap{}
heap.Init(&h)

return SeatManager{
h: h,
idx: 0,
}
}

func (this *SeatManager) Reserve() int {
if this.h.Len() > 0 {
return heap.Pop(&this.h).(int)
} else {
this.idx++
return this.idx
}
}

func (this *SeatManager) Unreserve(seatNumber int) {
heap.Push(&this.h, seatNumber)
}
```