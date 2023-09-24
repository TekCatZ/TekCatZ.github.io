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

## Reachable object

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

## Các thuật toán trong garbage collector

Về cơ bản những thuật toán theo phương pháp tracing sẽ có các bước như duyệt qua các object đang có trong chương trình và kiểm tra xem có reachable hay không để từ đó thực hiện bước tiếp theo là thu dọn những unreachable object và có thể tối ưu lại vùng nhớ nếu cần thiết. Đó cũng chính là lý do mà nó được gọi là tracing do từ bước dò tìm mà ra. Nhược điểm của phương pháp này sẽ không thu dọn vùng nhớ liên tục theo thời gian thực mà sẽ chỉ thu dọn khi thỏa mãn những điều kiện nhất định (nếu các bạn không nhớ có thể xem lại ở [phần 1](https://tekcatz.com/posts/garbage_collection/part_1/#gi%E1%BA%A3i-ph%C3%B3ng-v%C3%B9ng-nh%E1%BB%9B)).

### Naïve mark and Sweep

Một trong những thuật toán cơ bản và dễ hiểu nhất của tracing collector là Naïve mark and sweep (đánh dấu và quét dọn). Naïve ở đây có thể hiểu theo nghĩa giống như chúng ta chạy thuật toán theo cách đơn giản nhất mà chúng ta cũng có thể tự làm được. Do đó nó mới dễ hiểu và ngoài ra cũng có một số biến thể cho thuật toán mark and sweep này.

Đầu tiên, mỗi object trong vùng nhớ sẽ có 1 flag (1 bit là đủ). Flag này sẽ do garbage collector sử dụng chứ chúng ta không làm gì với nó cả và flag này sẽ luôn luôn ở trạng thái 0 trừ khi đang trong quá trình thu dọn vùng nhớ.

1. Bắt đầu quá trình thu dọn vùng nhớ, bước 1 là mark, tức là đánh dấu. Ở bước này garbage collector sẽ duyệt qua toàn bộ cây liên kết giữa các object. Thuật toán được sử dụng để duyệt cây là DFS (ai chưa biết DFS là gì thì có thể đọc ở [đây](https://en.wikipedia.org/wiki/Depth-first_search)) và những object nào có đường đi từ root tới nó thì sẽ được bật flag lên 1, tức là đang được sử dụng (reachable).
2. Bước tiếp theo là sweep, tức là dọn dẹp. Ở bước này vùng nhớ sẽ được scan qua để kiểm tra qua các ô nhớ còn free hay đã sử dụng. Nếu đã sử dụng thì sẽ kiểm tra flag của object đang nằm ở ô nhớ đó. Lưu ý rằng vùng nhớ khi được scan ở đây không phải lúc nào cũng là toàn bộ vùng nhớ heap mà như mình đã đề cập ở phần trước thì nếu vùng nhớ được chia thành các generation thì sẽ tối ưu cho việc dọn dẹp hơn. Và, những object nào không có bật flag là đang được sử dụng thì sẽ bị thu dọn, còn những object có flag được bật thì flag sẽ được clear về 0 để chuẩn bị cho chu kỳ dọn dẹp tiếp theo.

Thuật toán này dễ hiểu, dễ tiếp cận và đơn giản nhưng do đơn giản quá nên nó cũng có vài nhược điểm lớn như sau:
1. Đầu tiên đó là trong suốt cả quá trình thì toàn bộ chương trình sẽ bị tạm dừng lại, không được có bất kỳ object mới nào được khởi tạo vào lúc này. Nếu chương trình nhỏ thì không sao nhưng tưởng tượng những chương trình với rất nhiều object đang tồn tại bên trong thì sao? Thuật toán DFS có độ phức tạp là `O(V + E)` với `V` ở đây là số lượng object và `E` là liên kết giữa các object với nhau. Tức là càng nhiều object và càng nhiều liên kết giữa chúng thì việc chạy đi dọn dẹp sẽ chậm dần theo tuyến tính. Và mỗi lần dọn dẹp thì hệ thống phải tạm ngưng toàn bộ nên thời gian dọn dẹp chậm thì hệ thống bị tạm ngưng cũng lâu và có thể gây ra lag. Chưa kể đến việc trigger garbage collector chỉ diễn ra theo một số điều kiện nhất định nên có thể nói là nó khá khó dự đoán trước được khi nào hệ thống sẽ bắt đầu dọn dẹp. Bạn sẽ không muốn khi đang xài một hệ thống nào đó xong tự nhiên nó bắt đầu đơ ra dù bạn chả thao tác gì do hệ thống đang dọn dẹp vùng nhớ đâu nhỉ.
2. Ngoài ra, các chương trình thời kỳ trước khi còn chưa chia vùng nhớ thành các generation thì khi dọn dẹp vùng nhớ các garbage collector sẽ phải đi scan toàn bộ vùng nhớ heap chứ không chỉ 1 phần như hiện nay nên càng khiến cho việc dọn dẹp chậm đi rất nhiều. Tệ hơn nữa, với những máy tính thời xưa với dung lượng vùng nhớ vật lý thấp thì sẽ có cơ chế paging hoặc swap tức là xài một phần vùng nhớ từ ổ đĩa cứng để hỗ trợ giảm tải cho vùng nhớ vật lý. Mà các bạn biết rồi đấy, ổ cứng dù là SSD cũng không nhanh bằng ram được (không tính cái unified memory trên Mac nhé hihi), mà thời xưa toàn là ổ HDD nên càng chậm ác nữa nên garbage collector mà scan qua HDD thì thôi chậm khỏi nói.
3. Các object sau khi bị đánh dấu là unreachable sẽ được garbage collector dọn dẹp in-place tại chỗ, nghe thì có vẻ khá là nhanh nhưng thực tế điều này lại gây ra ảnh hưởng lớn vì nó có thể gây ra phân mảnh vùng nhớ như mình đã đề cập ở bài trước. Với các ứng dụng nhỏ thì việc phân mảnh một chút cũng không có vấn đề gì nhưng có rất nhiều ứng dụng đòi hỏi memory workload rất cao như các hệ thống in-mem cache (cache trên vùng nhớ vật lý thay vì trong ổ đĩa) thì việc phân mảnh này là khá là tối kỵ.

### Mark and Compact

Thuật toán mark and compact (đánh dấu và thu gom) là một biến thể từ mark and sweep. Về bản chất thì thuật toán này vẫn kế thừa được nhiều đặc tính từ thuật toán gốc của nó như đơn giản, dễ hiểu, cực kỳ chậm và không tối ưu. Thế nó có gì hơn người đi trước của nó nếu nó vẫn chậm và không tối ưu? Đó là nó sẽ giúp chúng ta giải quyết được nhược điểm số 3 gây phân mảnh vùng nhớ của thuật toán mark and sweep. Mark and compact có bước scan và đánh dấu các object cũng tương tự như mark and sweep. Điểm khác biệt là ở bước 2, thay vì dọn dẹp những unreachable object tại chỗ thì thuật toán sẽ đi copy những reachable object để xếp sát lại vào nhau và đẩy lên đầu vùng nhớ, và quá trình này còn được gọi là compact hoặc moving. Nó giống như việc các bạn đi siêu thị và đang chờ xếp hàng thì bỗng người phía trước rời hàng đi, lúc này theo lẽ tự nhiên thì các bạn sẽ dồn lên phía trên đứng vào vị trí còn trống đó và những người phía sau cũng dồn lên tương tự.

Được rồi, các bạn sẽ thắc mắc là nếu nó dời các reachable object như thế thì chả phải sẽ tốn kém thời gian hơn nhiều sao? Đúng, nó sẽ chậm hơn so với việc thu dọn tại chỗ, nhưng nó sẽ tốt hơn về mặt lâu dài. Các hệ thống lớn thường có thời gian hoạt động (available) rất lâu ví dụ như các server chẳng hạn và trong suốt quá trình hoạt động thì hệ thống sẽ sinh và hủy không biết bao nhiêu object. Với phương pháp dọn dẹp tại chỗ, việc dọn object sẽ gây ra phân mảnh vùng nhớ và nếu việc dọn dẹp này diễn ra nhiều lần trong một khoảng thời gian rất dài thì cái vùng nhớ chắc sẽ phân mảnh nát bét luôn và hệ thống sẽ không còn đủ vùng nhớ để khởi tạo object mới (xem lại bài 1 nếu các bạn quên mất lý do không đủ). Do đó việc sử dụng phương pháp di dời các reachable object lại sát với nhau để tạo thêm khoảng trống to hơn trên vùng nhớ lại rất hợp lý vì nó giúp hệ thống ổn định hơn trong thời gian lâu dài và các thuật toán đời sau cũng sử dụng phương pháp này thay vì dọn dẹp tại chỗ.

### Tri-color marking

Tri-color marking, tạm dịch là đánh dấu 3 màu sử dụng cơ chế chia các object vào các nhóm màu tương ứng nhằm cải thiện các vấn đề về hiệu năng ở phương pháp mark and sweep thông thường. Với phương pháp này, đầu tiên chúng ta sẽ có 3 màu sau:
* Trắng: những object trong nhóm màu trắng này là những object chưa được scan qua hoặc đã được xác nhận là unreachable object.
* Xám: những object trong nhóm màu xám này là những object đang được đánh dấu để chờ đến lượt kiểm tra và thường sẽ được đặt vào 1 hàng đợi (queue).
* Đen: những object trong nhóm màu đen là đã được kiểm tra và là reachable object nên không cần kiểm tra gì thêm.

Các bước chạy của thuật toán này như sau:

1. Khởi tạo: đưa tất cả các object vào nhóm màu trắng để đánh dấu rằng những đối tượng này chưa được kiểm tra.
2. Thuật toán sẽ đi từ các object ở root, đưa sang nhóm màu xám để đánh dấu cần kiểm tra. Ngoài ra khi đưa vào nhóm màu xám thì cũng sẽ đưa vào hàng đợi để chờ kiểm tra.
3. Tiếp đến, thuật toán sẽ lấy các object xám từ hàng đợi ra, đưa vào nhóm màu đen, tức là reachable object và đã kiểm tra rồi. Ngoài ra những object có liên kết với object màu đen này cũng sẽ được đánh dấu là màu xám và đưa vào hàng đợi để chờ scan.
4. Thuật toán kết thúc khi hàng đợi của màu xám rỗng, lúc này chúng ta có được 1 tập hợp các object màu đen đã được kiểm tra là reachable và những object còn sót lại trong nhóm màu trắng là unreachable. Nhiệm vụ của garbage collector lúc này chỉ là đi dọn hết các object nhóm màu trắng.

Nếu các bạn thấy các bước chạy có vẻ quen thì chứng tỏ bạn cày thuật toán cũng khá nhiều vì đây là cơ chế của thuật toán BFS, dùng để duyệt cây theo chiều rộng.

Lúc này các bạn sẽ thắc mắc: mark and sweep xài DFS, với độ phức tạp là `O(V + E)`, còn tri-color marking xài BFS và độ phức tạp cũng tương tự là `O(V + E)`. Nên theo lý thuyết thời gian chạy cả hai phải tương đồng nhau do ta đều phải duyệt qua toàn bộ các object, vậy thì tại sao tri-color marking lại có thể tốt hơn so với mark and sweep được?

## Tài liệu tham khảo

Trong quá trình viết bài mình đã tham khảo qua các nguồn sau:
* https://en.wikipedia.org/wiki/Tracing_garbage_collection
* https://en.wikipedia.org/wiki/Depth-first_search
* https://rebelsky.cs.grinnell.edu/Courses/CS302/99S/Presentations/GC/
