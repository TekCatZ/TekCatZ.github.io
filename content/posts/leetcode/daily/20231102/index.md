---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 26.10.2023 - 2265. Count Nodes Equal to Average of Subtree"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 2265. Count Nodes Equal to Average of Subtree"
showToc: true
date: 2023-11-01
---

## Bài toán

Link đến bài toán: [2265. Count Nodes Equal to Average of Subtree](https://leetcode.com/problems/count-nodes-equal-to-average-of-subtree)

## Phân tích

Bài này đơn giản ta duyệt cây theo `post-order` và trả về tổng, cùng số số lượng node của cây con đó sau đó so sánh với node hiện tại.

## Giải thuật

```go
func averageOfSubtree(root *TreeNode) int {
	count := 0
	var dfs func(root *TreeNode) []int
	dfs = func(root *TreeNode) []int {
		if root == nil {
			return []int{0, 0}
		}

		left := dfs(root.Left)
		right := dfs(root.Right)

		if (left[0]+right[0]+root.Val)/(left[1]+right[1]+1) == root.Val {
			count++
		}

		return []int{left[0] + right[0] + root.Val, left[1] + right[1] + 1}
	}

	dfs(root)
	return count
}
```