---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 10.11.2023 - 1759. Count Number of Homogenous Substrings"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1743. Restore the Array From Adjacent Pairs"
showToc: true
date: 2023-11-10
---

## Bài toán

Link đến bài toán: [1743. Restore the Array From Adjacent Pairs](https://leetcode.com/problems/restore-the-array-from-adjacent-pairs/)

## Phân tích

Hãy xem mỗi số như là một đỉnh trong đồ thị, mỗi cạnh nối 2 đỉnh là một cặp số liên tiếp trong mảng.
Ta có thể thấy rằng, mỗi đỉnh sẽ có đúng 2 cạnh nối với nó, ngoại trừ 2 đỉnh ở 2 đầu của đồ thị, mỗi đỉnh này chỉ có 1 cạnh nối với nó.
Vậy, ta chỉ cần chuyển các cặp số thành cạnh đồ thị, sau đó tìm ra đỉnh nào chỉ có 1 cạnh nối với nó, đó sẽ là đỉnh đầu tiên của mảng.
Sau đó thì DFS/BFS thôi.

## Giải thuật

```go
func restoreArray(adjacentPairs [][]int) []int {
	adj := make(map[int][]int)
	for _, v := range adjacentPairs {
		if _, ok := adj[v[0]]; ok {
			adj[v[0]] = append(adj[v[0]], v[1])
		} else {
			adj[v[0]] = append(make([]int, 0), v[1])
		}

		if _, ok := adj[v[1]]; ok {
			adj[v[1]] = append(adj[v[1]], v[0])
		} else {
			adj[v[1]] = append(make([]int, 0), v[0])
		}
	}

	ptr := 0
	for k, v := range adj {
		if len(v) == 1 {
			ptr = k
			break
		}
	}

	m := len(adj)
	res := make([]int, m)
	prev := ptr
	for i := 0; i < m; i++ {
		res[i] = ptr
		next := adj[ptr]
		if len(next) == 1 {
			prev = ptr
			ptr = next[0]
		} else {
			if next[0] == prev {
				prev = ptr
				ptr = next[1]
			} else {
				prev = ptr
				ptr = next[0]
			}
		}
	}
	return res
}
```