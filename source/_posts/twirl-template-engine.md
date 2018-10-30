---
title: Twirl template engine trong Play Framework
date: 2018-10-25 17:00:00
categories: 
    - play
tags: 
    - basic
---

## Template engine là gì ?

**Template engine** là một bộ xử lí template thường được viết bằng ngôn ngữ back-end .
 - Cho phép xử lí nghiệp vụ logic ở tầng view , giúp luồng code làm việc dễ dàng và linh hoạt hơn 
 - Giúp xử lí ,truy xuất , tính toán ,hiển thị hiệu quả dữ liệu ở trong phần giao diện .
 - Ở runtime, template engine thay thế những biến trong template với giá trị thực và chuyển đổi template thành file HTML gửi tới client.

<!-- more -->

### Ví dụ :
- Javascript thì có một số template engine như Mustache, EJS .
- PHP thì có Twig, Smarty ,Mustache.
- Java thì có  Apache Velocity,Thymeleaf, Apache FreeMaker,Groovy.

## Twirl là gì ?

### Khái niệm:
 **Twirl** là 1 bộ xử lí template dựa trên scala đi kèm với Play framework cho phép luồng code dễ dàng , chặt chẽ ,linh hoạt .

### Ví dụ:
  Khi bạn cần hiển thị một bảng với số lượng hàng chưa biết trước, sẽ rất vất vả khi mà bạn phải code thuần cả table đó. Thay cho việc đó, chúng ta dùng template engine. Template engine dựng khung table đủ các cột và 1 dòng chỉ định đến dữ liệu tương ứng, dữ liệu sẽ theo từng trường hợp hiện thị phù hợp.
```html 
 <table>
    <thead>
        <tr>         
            <th>Name</th>
            <th>Class</th>              
            <th>Address</th>
        </tr>
    </thead>
    <tbody>
      @for(student <- students){
          <tr>        
              <td>@student.name</td>
              <td>@student.className</td>      
              <td>@student.address</td>
          </tr>
      }
    </tbody>
  </table>
```
**Tại sao lại chọn twirl ?**

- [Chặt chẽ, biểu cảm , linh hoạt ] : nó giảm thiểu số lượng ký tự và tổ hợp phím được yêu cầu trong một tệp và cho phép luồng công việc mã hóa nhanh, linh hoạt.Không giống như hầu hết các cú pháp mẫu, bạn không cần phải ngắt mã hóa của mình để biểu thị rõ ràng các khối máy chủ trong HTML của bạn. Trình phân tích cú pháp đủ thông minh để suy ra điều này từ mã của bạn. Điều này cho phép một cú pháp thực sự nhỏ gọn và biểu cảm, sạch sẽ, nhanh chóng và thú vị để nhập.
- [Dễ học]  : nó cho phép bạn nhanh chóng trở nên năng suất, với tối thiểu các khái niệm. Bạn chỉ cần sử dụng các cấu trúc Scala đơn giản và tất cả các kỹ năng HTML hiện có của bạn
- [Không phải là một ngôn ngữ mới] :  Cho phép các nhà phát triển Scala sử dụng các kỹ năng ngôn ngữ Scala hiện có và cung cấp cú pháp đánh dấu mẫu cho phép tạo luồng công việc xây dựng HTML tuyệt vời.
- [Có thể chỉnh sửa trong bất kì trình soạn thảo văn bản nào] :nó không yêu cầu một công cụ cụ thể và cho phép bạn làm việc hiệu quả trong bất kỳ trình soạn thảo văn bản thuần tuý nào.

## Cú pháp của twirl ? 

**Play scala template** đơn giản là 1 file text đơn giản gồm những khối code scala nhỏ.
Templates có thể sinh ra bất kì định dạng text nào như là HTML, XML, CSV. Các template được biên dịch thành các hàm Scala chuẩn, theo một quy ước đặt tên đơn giản. Nếu bạn tạo một file template views/Application/index.scala.html , nó sẽ sinh ra một lớp views.html.Application.index gắn với  phương thức apply().

**Đây là một template đơn giản:**

```html
@(teachers: List[Teacher])

<h1>Welcome @customer.name!</h1>
<ul>
  @for(teacher <- teachers) {
    <li>@teacher.name</li>
  }
</ul>
```
Sau đó bạn có thể gọi nó từ bất kỳ mã Scala nào như bạn thường gọi một phương thức trên một lớp:

```scala
val content = views.html.Application.index(teachers);
```

Một scala template  là một file HTML với những câu lệnh scala được nhúng vào và bắt đầu bằng kí tự **@**.
Một template cũng là 1 hàm ,nó cần tham số và được khai báo ở dòng đầu tiên của file template.

#### Ví dụ:

```scala
@(teachers: List[Teacher])
@(title: String = "Home")
```

## NHỮNG TRƯỜNG HỢP HAY SỬ DỤNG 

#### Phép lặp

```html
<ul>
@for(p <- products) {
  <li>@p.name (@p.price)</li>
}
</ul>
```

#### Khối if

```html
@if(items.isEmpty) {
  <h1>Nothing to display</h1>
} else {
  <h1>@items.size items!</h1>
}
```
#### Tạo khối code có thể dùng lại
```html
@display(product: Product) = {
  @product.name (@product.price)
}

<ul>
@for(product <- products) {
  @display(product)
}
</ul>
```
#### Comment
```html
@********************
* This is a comment *
*********************@
```

#### Pattern matching
```html
@user match {
  case  Some(name) => {
    <span class=”admin”> Connect to (@name) </span>
    }
  case None => {
    <span> Not logged </span>
    }
}
```

#### Import 
```html
@import utils._
@import models.
```
#### LAYOUT

[1] Khai báo [views/main.scala.html] hoạt động như một layout template chính:
```html
@(title: String)(content: Html)
<!DOCTYPE html>
<html>
  <head>
    <title>@title</title>
  </head>
  <body>
    <section class="content">@content</section>
  </body>
</html>
```
[2] Như bạn thấy, mẫu này có hai tham số: title và block code HTML. Bây giờ chúng ta có thể  tái sử dụng nó từ một template khác [views/Application/index.scala.html]
```html
@main(title = "Home") {
  <h1>Home page</h1>
}
```
## Nhận xét về  độ tiện lợi của twirl
Twirl giúp tiết kiệm thời gian khi viết đi viết lại cấu trúc code phổ biến.

## Nguồn tham khảo:
https://www.playframework.com/documentation/2.5.x/ScalaTemplates
https://alvinalexander.com/scala/create-play-framework-template-functions-examples
https://www.slideshare.net/knoldus/knolx-pte


#### Tác giả: Nguyễn Đình Cường