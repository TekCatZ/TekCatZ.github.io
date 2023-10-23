---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 22.10.2023 - 342. Power of Four"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 342. Power of Four"
showToc: true
date: 2023-10-23
---

## Bài toán

Link đến bài toán: [342. Power of Four](https://leetcode.com/problems/power-of-four)

## Phân tích

Bài này khá dễ.

### Đệ quy/vòng lặp

Cách đơn giản nhất là ta sẽ lặp/đệ quy chừng nào `n` chia hết cho `4` và `n > 1`
với base case là `n == 1` (4<sup>0</sup> = 1).

```
f(n) = f(n/4) nếu n chia hết cho 4 và n > 1
f(n) = false nếu n không chia hết cho 4 hoặc n <= 1
f(1) = true
```

Với yêu cầu follow-up là không dùng đệ quy và vòng lặp,
ta có thể dùng một số kĩ thuật như `bit manipulation` để giải, hoặc dùng một số công thức toán học.

### Toán

Ta có thể dùng công thức sau để giải bài này:

```
n = 4^x
=> log4(n) = x
=> 1/2 * log2(n) = x
=> log2(n) / 2 = x
```

Vì `x` là số nguyên nên `log2(n) % 2 == 0` là điều kiện cần và đủ để `n` là lũy thừa của 4.
Để tính `log2(n)` ta có thể dùng công thức `log2(n) = log10(n) / log10(2)`.

Lưu ý: Một số ngôn ngữ cài đặt hàm log bằng vòng lặp.

### Bit manipulation

Ta nhận thấy:

```
4 = 0b100
16 = 0b10000
64 = 0b1000000
256 = 0b100000000
```

Các số này đều chỉ có duy nhất 1 bit 1 và bit 1 này luôn nằm ở vị trí chẵn. (Lập trình viên đếm từ 0, trừ khi bạn code Pascal hay Delphi)

Để kiểm tra xem một số khi biểu diễn ở dạng nhị phân có thỏa mãn điều kiện chỉ có duy nhất 1 bit, ta có thể dùng công thức sau:

```
n & (n - 1) == 0
```

Ví dụ:
```
    4 = 0b100
4 - 1 = 0b011
4 & 3 = 0b000
```

Với điều kiện: _bit 1 này luôn nằm ở vị trí chẵn._

Ta có thể dùng một số mà biểu diễn nhị phân của nó có bit 1 ở tất cả các vị trí lẻ.
Với 32 bit, ta dùng số (101010...)<sub>2<sub> = (0xAAAAAAAA)<sub>16<sub>.

Ngoài ra, còn một cách khác để kết hợp với điều kiện đầu (chỉ có duy nhất 1 bit 1).

4<sup>k</sup> = ((3+ 1)<sup>k</sup> = 3<sup>k</sup> + 3<sup>k</sup> + ... + 3<sup>k</sup> + 1<sup>k</sup>

4<sup>k</sup> % 3 = 1<sup>k</sup> = 1


## Giải thuật

### Đệ quy/vòng lặp

Cách này dễ quá mọi người tự làm nhé. /ᐠ - ˕ -マ Ⳋ

### Toán

```go
func isPowerOfFour(n int) bool {
    if n <= 0 {
        return false
    }
    log2 := math.Log2(float64(n))
    return math.Mod(log2, 2) == 0
}
```

### Bit manipulation

#### Cách 1

```go
func isPowerOfFour(n int) bool {
    if n <= 0 {
        return false
    }
    return n & (n - 1) == 0 && n & 0xAAAAAAAA == 0
}
```

#### Cách 2

```go
func isPowerOfFour(n int) bool {
    return n > 0 && n&(n-1) == 0 && (n-1)%3 == 0
}
```