---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 17.10.2023 - 1361. Validate Binary Tree Nodes"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1361. Validate Binary Tree Nodes"
showToc: true
date: 2023-10-17
---

## Bài toán

Link đến bài toán: [1361. Validate Binary Tree Nodes](https://leetcode.com/problems/validate-binary-tree-nodes)

## Phân tích

Bài này khó nhất là hiểu xem đề bài muốn gì.
Đề cho ta một list các node và cạnh của các node đó và muốn ta kiểm tra xem đó có phải là một cây nhị phân hợp lệ hay không.
Điều kiện để các node này tạo thành một cây nhị phân hợp lệ là:
- Có đúng 1 node là root
- Các node khác đều có đúng 1 node cha
- Các node không được tạo thành các chu trình

Để xác định 2 điều kiện sau, ta đơn giản kiểm ra số bậc trong (in-degree) của từng node là được:
- Nếu có 2 node có in-degree = 0 thì đó không phải là cây nhị phân hợp lệ
- Nếu có 1 node có in-degree > 1 thì đó không phải là cây nhị phân hợp lệ

Để kiểm tra điều kiện cuối cùng, ta sử dụng [Union-Find/Disjoint Set](https://en.wikipedia.org/wiki/Disjoint-set_data_structure) để kiểm tra xem các node có liên thông với nhau hay không.

## Giải thuật

### Union-Find

```go
type UnionFind struct {
	parent []int
	rank   []int
}

func (uf *UnionFind) init(n int) {
	uf.parent = make([]int, n)
	uf.rank = make([]int, n)
	for i := 0; i < n; i++ {
		uf.parent[i] = i
		uf.rank[i] = 1
	}
}

func (uf *UnionFind) find(x int) int {
	if uf.parent[x] == x {
		return x
	}
	uf.parent[x] = uf.find(uf.parent[x])
	return uf.parent[x]
}

func (uf *UnionFind) union(x, y int) bool {
	xp := uf.find(x)
	yp := uf.find(y)
	if xp == yp {
		return false
	}
	if uf.rank[xp] < uf.rank[yp] {
		uf.parent[xp] = yp
	} else if uf.rank[xp] > uf.rank[yp] {
		uf.parent[yp] = xp
	} else {
		uf.parent[yp] = xp
		uf.rank[xp]++
	}
	return true
}

func validateBinaryTreeNodes(n int, leftChild []int, rightChild []int) bool {
	inDegree := make([]int, n)
	for i := 0; i < n; i++ {
		if leftChild[i] != -1 {
			inDegree[leftChild[i]]++
		}
		if rightChild[i] != -1 {
			inDegree[rightChild[i]]++
		}
	}
	count := 0
	for i := 0; i < n; i++ {
		if inDegree[i] == 0 {
			count++
		}
	}
	if count != 1 {
		return false
	}

	isRootFound := false
	for i := 0; i < n; i++ {
		if inDegree[i] != 1 && isRootFound {
			return false
		} else if inDegree[i] == 0 {
			isRootFound = true
		}
	}

	uf := UnionFind{}
	uf.init(n)
	for i := 0; i < n; i++ {
		if leftChild[i] != -1 {
			if !uf.union(i, leftChild[i]) {
				return false
			}
		}
		if rightChild[i] != -1 {
			if !uf.union(i, rightChild[i]) {
				return false
			}
		}
	}

	root := uf.find(0)
	for i := 1; i < n; i++ {
		if uf.find(i) != root {
			return false
		}
	}

	return true
}
```

Độ phức tạp: **O(n).**
