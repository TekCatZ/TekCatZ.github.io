---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 29.10.2023 - 458. Poor Pigs"
tags: [ "leetcode", "algorithm" ]
summary: "Giải Leetcode 458. Poor Pigs"
showToc: true
date: 2023-10-29
---

## Bài toán

Link đến bài toán: [458. Poor Pigs](https://leetcode.com/problems/poor-pigs/)

## Phân tích

Bài này thuần túy là toán.../ᐠ - ˕ -マ Ⳋ 
Giả định ta chỉ có 1 lần thử, **minutesToTest / minutesToDie = 1**, và 1 con heo, thì ta thử được 2 lọ thuốc.
Vậy nếu có 2 con heo, ta có thể thử được 4 lọ thuốc, 3 con heo thì 8 lọ thuốc, 4 con heo thì 16 lọ thuốc, etc.
Khái quát hóa, ta có thể thử được **2^pigs** lọ thuốc với **pigs** con heo.

Nếu ta có **minutesToTest / minutesToDie = n**, thì ta có thể thử được **(n+1)^pigs** lọ thuốc với **pigs** con heo.
Vậy tức là ta cần tìm **pigs** sao cho **(n+1)<sup>pigs</sup> >= buckets**.
Vậy **pigs = ceil(log<sub>n+1</sub>(buckets))**.

## Giải thuật
Có 2 dòng thôi á, dễ lắm, thế công thức ở trên vào và xài hàm `log` có sẵn trong thư viện của từng ngôn ngữ là xong.
