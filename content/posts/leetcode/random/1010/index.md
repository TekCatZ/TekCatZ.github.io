---
author: ShadowCat
title: "Cùng giải Leetcode - 1010. Pairs of Songs With Total Durations Divisible by 60"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1010. Pairs of Songs With Total Durations Divisible by 60"
showToc: true
date: 2023-10-16
---

## Bài toán

Link đến bài toán: [1010. Pairs of Songs With Total Durations Divisible by 60](https://leetcode.com/problems/pairs-of-songs-with-total-durations-divisible-by-60/)

## Phân tích

Nó gần giống bài này này:
[1. Two Sum](https://leetcode.com/problems/two-sum/description/)

Ta có thể brute force, hoặc dùng hash table để giải bài này.

Quan sát thấy, `map[60 - x%60]` sẽ là số bài hát có thể ghép với x để tổng thời gian là bội của 60.
Vậy kết quả sẽ là tổng giá trị của `map[x%60]` với `x` là thời gian của bài hát.


## Giải thuật

### Brute force

```go
func numPairsDivisibleBy60(time []int) int {
    res := 0
    for i := 0; i < len(time); i++ {
        for j := i + 1; j < len(time); j++ {
            if (time[i] + time[j]) % 60 == 0 {
                res++
            }
        }
    }
    return res
}
```

Độ phức tạp: **O(n<sup>2</sup>).**

### Hash table

```go
func numPairsDivisibleBy60(time []int) int {
    var res int
    var m = make(map[int]int)
    for _, v := range time {
        res += m[(60-v%60)%60]
        m[v%60]++
    }
    return res
}
```

Độ phức tạp: **O(n).**