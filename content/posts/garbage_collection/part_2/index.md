---
author: Ming
title: "Garbage Collection - bạn có thật sự hiểu về nó? Phần 2"
tags: ["programming languages", "garbage_collection"]
summary: "Ở phần trước chúng ta đã tìm hiểu qua về vùng nhớ và tổng quan về garbage collection. Sang phần này chúng ta sẽ xem qua thuật toán mà garbage collector sử dụng để dọn vùng nhớ nhé."
date: 2023-09-18
draft: true
---

Ở phần trước chúng ta đã được ôn tập lại các khái niệm cơ bản về vùng nhớ cũng như biết được thêm cách mà chương trình phân chia vùng nhớ heap để tối ưu hóa việc dọn dẹp vùng nhớ như thế nào. Trong phần này chúng ta sẽ tìm hiểu sâu vào cách thức hay thuật toán mà garbage collector sử dụng để dọn dẹp vùng nhớ.

Về cơ bản có 2 phương pháp chính được sử dụng hiện nay:
* Tracing: hay còn được gọi là truy vết object. Công việc chính sẽ là đi dò tìm xem những object nào là reachable (nếu bạn quên reachable objects là gì thì có thể xem lại [bài trước](https://tekcatz.com/posts/garbage_collection/part_1/)) hiện đang có trong chương trình dựa theo các tham chiếu (references) bắt đầu từ gốc (root). Những object nào không phải là reachable thì sẽ mặc định là không còn dùng nữa và sẽ đưa đi thu dọn.
* Reference counting: như cái tên đã nói lên tất cả, phương pháp này công việc chính là đi đếm số lượng reference đang trỏ vào object đó và lưu trữ bộ đếm trong quá trình chương trình chạy. Khi bộ đếm cho 1 object về 0 tức là object đó đã hết tham chiếu đến nó, nghĩa là nó không còn được sử dụng nữa và sẽ được thu hồi ngay lúc đó.

Trong bài này thì mình sẽ đi vào phương pháp tracing trước và bài sau mình sẽ giới thiệu qua về reference counting.

## Tham chiếu của object

### Object

## Tài liệu tham khảo

Trong quá trình viết bài mình đã tham khảo qua các nguồn sau:
* https://en.wikipedia.org/wiki/Tracing_garbage_collection
