---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 11.11.2023 - 2642. Design Graph With Shortest Path Calculator"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 2642. Design Graph With Shortest Path Calculator"
showToc: true
date: 2023-11-11
---

## Bài toán

Link đến bài toán: [2642. Design Graph With Shortest Path Calculator](https://leetcode.com/problems/design-graph-with-shortest-path-calculator/description/)

## Phân tích

Bài này tag `hard` thôi chứ nó dễ ẹc à.

n <= 100, max 100 call.

=> Spam [dijkstra](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) cho mình.

## Giải thuật

```go
type Graph struct {
	adj [][]int
}

func Constructor(n int, edges [][]int) Graph {
	graph := Graph{adj: make([][]int, n)}
	for i := 0; i < n; i++ {
		graph.adj[i] = make([]int, n)
	}
	for _, edge := range edges {
		graph.adj[edge[0]][edge[1]] = edge[2]
	}
	return graph
}

func (this *Graph) AddEdge(edge []int) {
	this.adj[edge[0]][edge[1]] = edge[2]
}

func (this *Graph) ShortestPath(node1 int, node2 int) int {
	d := dijkstra(*this, node1, node2)
	if d == math.MaxInt32 {
		return -1
	}
	return d
}

func dijkstra(graph Graph, start int, end int) int {
	n := len(graph.adj)
	visited := make([]bool, n)
	dist := make([]int, n)
	for i := 0; i < n; i++ {
		dist[i] = math.MaxInt32
	}
	dist[start] = 0
	for i := 0; i < n; i++ {
		u := minDistance(dist, visited)
		visited[u] = true
		for v := 0; v < n; v++ {
			if !visited[v] && graph.adj[u][v] != 0 && dist[u] != math.MaxInt32 && dist[u]+graph.adj[u][v] < dist[v] {
				dist[v] = dist[u] + graph.adj[u][v]
			}
		}
	}
	return dist[end]
}

func minDistance(dist []int, visited []bool) int {
	min := math.MaxInt32
	minIndex := -1
	for i := 0; i < len(dist); i++ {
		if !visited[i] && dist[i] <= min {
			min = dist[i]
			minIndex = i
		}
	}
	return minIndex
}
```