---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 15.10.2023 - 1269. Number of Ways to Stay in the Same Place After Some Steps"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 1269. Number of Ways to Stay in the Same Place After Some Steps"
showToc: true
date: 2023-10-15
---

## Bài toán

Link đến bài toán: [1269. Number of Ways to Stay in the Same Place After Some Steps](https://leetcode.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps/)

## Phân tích

Tại mỗi bước, ta có thể chọn đi sang trái, phải, hay đứng yên miễn là không vượt quá giới hạn `arrLen`.
Ta đặt `f(cur, remain)` là số cách để đứng ở vị trí `cur` sau khi đi `remain` bước.
Có 3 trường hợp có thể xảy ra tại bước thứ `remain`:
- Đứng yên: `f(cur, remain) = f(cur, remain - 1)`
- Đi sang trái: `f(cur, remain) = f(cur - 1, remain - 1)` khi `cur > 0`
- Đi sang phải: `f(cur, remain) = f(cur + 1, remain - 1)` khi `cur < arrLen - 1`

Base case:
- cur = 0, remain = 0: `f(cur, remain) = 1` (đứng yên tại vị trí 0 sau 0 bước)
- cur > 0, remain = 0: `f(cur, remain) = 0` (không còn cách nào để đứng tại vị trí `cur = 0` sau 0 bước)

Nếu để ý, ta có thể nhận ra `arrLen` không ảnh hưởng đến độ phức tạp của bài toán. /ᐠ - ˕ -マ Ⳋ

## Giải thuật

### Đệ quy

```go
const mod = 1e9 + 7

func numWays(steps int, arrLen int) int {
    if arrLen > steps {
        arrLen = steps + 1
    }
    f := func(cur, remain int) int {
       if remain == 0 {
           if cur == 0 {
               return 1
           }
           return 0
       }
       res := f(cur, remain - 1)
       if cur > 0 {
           res += f(cur - 1, remain - 1) % mod
       }
       if cur < arrLen - 1 {
           res += f(cur + 1, remain - 1) % mod
       }
       return res % mod
    }
    return f(0, steps)
}
```

Độ phức tạp: **O(3<sup>n</sup>).**
Ta có thể tối ưu bằng một **map** nhớ lại các giá trị đã tính. Các bạn tự implement nhé. /ᐠ｡ꞈ｡ᐟ\

### Dynamic programming

Nếu ta sử dụng một mảng 2 chiều `dp[i][j]` để lưu số cách để đứng tại vị trí `i` sau khi đi `j` bước, thì với constraint của bài toán.
Ta sẽ gặp MLE (Memory Limit Exceeded). Như đã nói ở trên, `arrLen` không ảnh hưởng đến độ phức tạp của bài toán.
Vậy nên ta sẽ chỉ cần lưu lại hàng trước đó của `dp` là được. (˵ •̀ ᴗ - ˵ ) ✧

```go
const mod = 1e9 + 7

func numWays(steps int, arrLen int) int {
    n := min(arrLen, steps)
    dp := make([]int, n)
    last := make([]int, n)

    last[0] = 1
    for i := 1; i <= steps; i++ {
        for j := 0; j < n; j++ {
            dp[j] = last[j]
            if j > 0 {
                dp[j] = (dp[j] + last[j-1]) % mod
            }
            if j < n-1 {
                dp[j] = (dp[j] + last[j+1]) % mod
            }
        }
        copy(last, dp)
    }

    return dp[0]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

Độ phức tạp: **O(n<sup>2</sup>).**

Tối ưu: Cách giải trên còn có thể tối ưu thêm, ví dụ như ta biết rằng ta chỉ cần tính đến steps / 2 bước là đủ.
Các bạn tự tối ưu thêm nhé. Tác giả đi ngủ đây. (=🝦 ﻌ 🝦=)