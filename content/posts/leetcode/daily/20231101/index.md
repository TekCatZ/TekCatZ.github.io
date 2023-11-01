---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 26.10.2023 - 501. Find Mode in Binary Search Tree"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 501. Find Mode in Binary Search Tree"
showToc: true
date: 2023-11-01
---

## Bài toán

Link đến bài toán: [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/)

## Phân tích

Bài này có hai cách giải, hoặc là dùng `hashmap` hoặc dựa vào tính chất của cây nhị phân tìm kiếm và sử dụng `inorder traversal`.

Với cách dùng `hashmap` thì ta sẽ duyệt qua cây và đếm số lần xuất hiện của các giá trị, sau đó tìm các giá trị có số lần xuất hiện nhiều nhất là được.

Còn với cách dùng `inorder traversal`, ta để ý rằng đề bài cho ta một cây BST.
Tính chất của cây BST đảm bảo rằng, khi ta duyệt cây bằng `inorder traversal` (BFS), ta sẽ duyệt các giá trị theo thứ tự được sắp xếp (trong bài này là tăng dần).

## Giải thuật

### Giải thuật 1: Dùng `hashmap`

1. Khởi tạo một `hashmap` để đếm số lần xuất hiện của các giá trị.
2. Duyệt cây và đếm số lần xuất hiện của các giá trị.
3. Tìm các giá trị có số lần xuất hiện nhiều nhất.
4. Trả về kết quả.

Này dễ nên mọi người tự cài đặt nhớ /ᐠ - ˕ -マ Ⳋ

### Giải thuật 2: BFS

1. Khởi tạo một `Set` để lưu các giá trị.
2. Khởi tạo biến `curNum`, `curStreak`, `maxStreak` để lưu giá trị hiện tại, số lần xuất hiện liên tiếp của giá trị hiện tại, số lần xuất hiện liên tiếp nhiều nhất.
3. Khởi tạo queue `q` để duyệt BFS, thêm node gốc vào `q`, giá trị ban đầu của `curNum` là giá trị của node gốc, maxStreak = 1, curStreak = 1.
4. Duyệt BFS:
   1. Lấy node đầu tiên ra khỏi `q` và so sánh giá trị của node đó với `curNum`:
      1. Nếu giá trị của node đó bằng `curNum` thì tăng `curStreak` lên 1.
      2. Nếu giá trị của node đó khác `curNum` thì gán `curNum` bằng giá trị của node đó, `curStreak` bằng 1, `maxStreak` bằng 1.
   2. Nếu `curStreak` bằng `maxStreak` thì thêm `curNum` vào `Set`.
   3. Nếu `curStreak` lớn hơn `maxStreak` thì gán `maxStreak` bằng `curStreak`, xóa hết các giá trị trong `Set` và thêm `curNum` vào `Set`.
   4. Nếu node đó có node con trái thì thêm node con trái vào `q`.
   5. Nếu node đó có node con phải thì thêm node con phải vào `q`.
5. Trả về kết quả.

```go
func findMode(root *TreeNode) []int {
	q := make([]*TreeNode, 0)
	resMap := make(map[int]bool)
	q = append(q, root)

	maxStreak := 0
	curNum := root.Val
	curStreak := 0
	for len(q) > 0 {
		node := q[0]
		q = q[1:]
		if node.Val == curNum {
			curStreak++
		} else {
			curStreak = 1
			maxStreak = 1
			curNum = node.Val
		}

		if curStreak == maxStreak {
			resMap[node.Val] = true
		} else if curStreak > maxStreak {
			resMap = make(map[int]bool)
			resMap[node.Val] = true
		}

		if node.Left != nil {
			q = append(q, node.Left)
		}

		if node.Right != nil {
			q = append(q, node.Right)
		}
	}

	res := make([]int, 0)
	for k, _ := range resMap {
		res = append(res, k)
	}

	return res
}
```