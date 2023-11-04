---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 01.11.2023 - 501. Find Mode in Binary Search Tree"
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
Tính chất của cây BST đảm bảo rằng, khi ta duyệt cây bằng `inorder traversal` (BFS/DFS), ta sẽ duyệt các giá trị theo thứ tự được sắp xếp (trong bài này là tăng dần).
Từ đây có thể giải bằng cách convert cây sang một mảng và duyệt mảng để tìm các giá trị có số lần xuất hiện nhiều nhất, hoặc xử lý thẳng trên cây khi duyệt.

## Giải thuật

### Giải thuật 1: Dùng `hashmap`

1. Khởi tạo một `hashmap` để đếm số lần xuất hiện của các giá trị.
2. Duyệt cây và đếm số lần xuất hiện của các giá trị.
3. Tìm các giá trị có số lần xuất hiện nhiều nhất.
4. Trả về kết quả.

Này dễ nên mọi người tự cài đặt nhớ /ᐠ - ˕ -マ Ⳋ

### Giải thuật 2: DFS

1. Khởi tạo một biến `maxStreak` để lưu giá trị lớn nhất của `curStreak` (số lần xuất hiện nhiều nhất của một giá trị).
2. Khởi tạo một biến `curStreak` để lưu số lần xuất hiện của giá trị hiện tại.
3. Khởi tạo một biến `lastNum` để lưu giá trị của node trước đó.
4. Khởi tạo một mảng `res` để lưu kết quả.
5. Khởi tạo một hàm `dfs` để duyệt cây:
    1. Nếu node hiện tại là `nil` thì thoát.
    2. Gọi đệ quy `dfs` với node con bên trái.
    3. Nếu giá trị của node hiện tại bằng với `lastNum`:
        1. Tăng `curStreak` lên 1.
    4. Nếu không:
        1. Gán `curStreak` bằng 1.
        2. Gán `lastNum` bằng giá trị của node hiện tại.
    5. Nếu `curStreak` > `maxStreak`:
        1. Gán `maxStreak` = `curStreak`.
        2. Gán `res` bằng một mảng chứa giá trị của node hiện tại.
    6. Nếu `curStreak` == `maxStreak`:
        1. Thêm giá trị của node hiện tại vào `res`.
    7. Gọi đệ quy `dfs` với node con bên phải.

```go
func findMode(root *TreeNode) []int {
	maxStreak, curStreak, lastNum := 0, 0, 0
	res := make([]int, 0)
	var dfs func(root *TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		if root.Val == lastNum {
			curStreak++
		} else {
			curStreak = 1
			lastNum = root.Val
		}
		if curStreak > maxStreak {
			maxStreak = curStreak
			res = []int{root.Val}
		} else if curStreak == maxStreak {
			res = append(res, root.Val)
		}
		dfs(root.Right)
	}

	dfs(root)
	return res
}
```