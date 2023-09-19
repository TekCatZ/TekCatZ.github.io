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

### Reachable objects

Chúng ta đã nói qua nhiều lần về reachable/unreachable objects nhưng chưa thật sự đi sâu vào nó. Về định nghĩa, một object được xem là reachable nếu nó được tham chiếu đến (trực tiếp hoặc gián tiếp) từ các reachable objects khác. Có 2 cách sau:
* Reachable objects từ root: root ở đây có thể xem là các entry cho những vùng nhớ trong chương trình, như call stack (bao gồm việc khai báo biến cục bộ và các tham số truyền vào hàm khi hàm được gọi) và các biến toàn cục.
* Các object có tham chiếu đến từ các reachable objects khác.

Khi gọi 1 object là "rác" vùng nhớ, chúng ta thường đề cập nó như là một syntactic garbage, tức là "rác" chính xác chuẩn 100% về mặt cú pháp. Giải thích qua tiếng Việt có vẻ hơi lạ nên mình sẽ có 1 ví dụ minh họa như sau:

```javascript
let x = new Foo();
x = new Bar();
```

Ở ví dụ trên, ban đầu object x được khai báo là kiểu `Foo` nhưng về sau lại được gán lại bằng một object kiểu `Bar` mới nên ở đây chúng ta có thể xác định chính xác object `Foo` ban đầu đã không còn nữa do không còn tham chiếu đến nó. Như vậy, syntactic garbage là những object mà chúng ta biết chính xác là nó đã mất tham chiếu hoàn toàn và trở thành "rác" cần được dọn.

Ngoài syntactic garbage thì chúng ta còn có semantic garbage, tức là "rác" về mặt ngữ nghĩa, logic.

```javascript
let x = new Foo();
if (params[_destroy]) {
  x = null;
}
```

Ở đây, **về mặt ngữ nghĩa**, ta có thể thấy rõ x có khả năng sẽ trở thành rác và chuyện đó sẽ xảy ra khi `params[_destroy]` trả ra truthy. Như vậy, semantic garbage có nghĩa là object đó có thể trở thành rác và cần phải được dọn dẹp nếu thỏa mãn điều kiện về mặt ngữ nghĩa hay logic trong code.

### Tham chiếu mạnh và yếu (strong and weak references)

## Tài liệu tham khảo

Trong quá trình viết bài mình đã tham khảo qua các nguồn sau:
* https://en.wikipedia.org/wiki/Tracing_garbage_collection
