---
author: Ming
title: "Garbage Collection - bạn có thật sự hiểu về nó?"
tags: ["programming languages"]
draft: true
summary: "Khi còn học trên giảng đường, chúng ta thường được dạy rằng những ngôn ngữ như Java, Python, C#,... có tích hợp sẵn Garbage Collection để tự dọn vùng nhớ đã được cấp phát cho các object. Tuy nghe nhiều là thế nhưng liệu các bạn có biết cơ chế hoạt động đằng sau nó chưa?"
---

Khi còn học trên giảng đường, các bạn thường được nghe giảng rằng một trong những điểm khác biệt lớn giữa những ngôn ngữ như Java, Python, JS, C#,... so với C/C++ đó là những ngôn ngữ đó có tích hợp sẵn Garbage Collection, tức là khi chúng ta khai báo các object thì chúng ta sẽ không cần phải gọi `delete` để dọn vùng nhớ được cấp phát cho các object thủ công như C/C++ nữa mà những Garbage Collection đó sẽ tự làm những công việc đó tự động thay cho chúng ta. Cơ mà, biết là Garbage Collection rất tiện nhưng liệu các bạn có thật sự biết nó làm gì bên trong để dọn vùng nhớ cho chúng ta không? Hãy cùng tìm hiểu trong bài viết này nhé.
