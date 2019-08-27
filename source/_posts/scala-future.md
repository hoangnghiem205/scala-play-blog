---
title: Xử lý bất đồng bộ trong Scala
date: 2019-08-27 10:00:00
categories: 
    - scala
tags: 
    - basic
---

## Đồng bộ và bất đồng bộ: 

{% asset_img async.png %}

Với đồng bộ, luồng xử lý sẽ đợi cho tiến trình A xử lý hoàn tất rồi mới xử lý tiếp tiến trình B. 
Với bất đồng bộ, luồng xử lý chính sẽ chia làm 2 luồng: xử lý tiến trình A và xử lý tiến trình B luôn mà không đợi tiến trình A kết thúc.
Trong Scala Play, bất đồng bộ thường được bọc trong Future

<!-- more -->

Sử dụng bất đồng bộ giúp tăng hiệu suất, có thể thực hiện tiến trình khác trong khi chờ kết quả từ tiến trình đang chạy.

Một số trường hơp cần đồng bộ: 
- Khi cần kết quả từ tiến trình A để xử lý tiến trình B thì chúng ta sẽ phải xử lý bất đồng bộ ở tiến trình A để luồng xử lý chính xử lý tiến trình A xong mới xử lý tiếp tiến trình B.
- Khi làm việc với slick trong `Play framework` kết quả trả về thường sẽ được bọc trong Future. Do vậy, nếu muốn sử dụng kết quả để tính toán chúng ta sẽ phải đồng bộ hóa mọi tiến trình có sử dụng kết quả đấy.
## Một số cách lấy giá trị trong Future:

+ `for-yield`: giống for nhưng có thêm bộ đệm, mỗi vòng for giá trị sẽ được add vào bộ đệm đến khi hết vòng for thì kết quả trả về bộ đệm. Nếu trong vòng for lệnh đầu tiên chạy bất đồng bộ thì kết quả trả về trong yield sẽ được bọc trong Future và mỗi lệnh trong `for-yield` sẽ chạy đồng bộ.
```scala
// HÀM TRẢ VỀ FUTURE
  def all(headerID: String): Future[Seq[OrderDetail]]
// SỬ DỤNG for-yield ĐỂ LẤY GIÁ TRỊ
    for {
        orderDetails <- all(headerID)
    } yield {
        Ok
}

```
+ `map` : Tác dụng gần giống `for-yield` là lấy giá trị từ bất đồng bộ. `map` chỉ được sử dụng khi có duy nhất một truy vấn bất đồng bộ và kết quả trả về sẽ được bọc trong Future.

```scala
// HÀM TRẢ VỀ FUTURE
  def find(detailID: String): Future[Option[OrderDetail]]

// SỬ DỤNG map ĐỂ LẤY GIÁ TRỊ
    find(orderDetailId).map { orderDetail => 
        OK
    }
```


+ `flatMap`: Là sự kết hợp giữa `map` và `flatten`. Sử dụng `flatMap` trong trường hợp có nhiều hàm bất đồng bộ lồng nhau. Kết quả trả về không được bọc trong Future. Nếu biến khởi tạo là bất đồng bộ thì kết quả trả về phải được bọc trong Future.

```scala
// HÀM TRẢ VỀ FUTURE
  def find(detailID: String): Future[Option[OrderDetail]]

// SỬ DỤNG flatMap ĐỂ LẤY GIÁ TRỊ
    find(orderDetailId).flatMap { orderDetail => 
        Future.successful(OK)
    }
```

+ `async,await`: Kết quả trả về trong `async` được bọc trong Future. Sử dung hàm `await()` để đồng bộ các tiến trình chạy bất đồng bộ trong `async`

```scala
import concurrent.Await
import async.Async._

// HÀM TRẢ VỀ FUTURE
      def find(detailID: String): Future[Option[OrderDetail]]
// SỬ DỤNG async,await ĐỂ LẤY GIÁ TRỊ
    async {
        await(find(detailID))
    }
```

+ `Await.result`: Kết hợp duration khi sử dụng luồng chính sẽ đợi tiến trình bất đồng bộ xử lý trong một khoảng thời gian được nhập vào. Kết quả trả về không được bọc trong Future.
```scala
import scala.concurrent.duration._
import scala.language.postfixOps

// HÀM TRẢ VỀ FUTURE
      def find(detailID: String): Future[Option[OrderDetail]]
// SỬ DỤNG Await.result ĐỂ LẤY GIÁ TRỊ
    Await.result(find(detailID, Duration.Inf))
```
## Nguồn tham khảo:

https://docs.scala-lang.org/sips/async.html

https://www.playframework.com/documentation/2.7.x/ScalaAsync
