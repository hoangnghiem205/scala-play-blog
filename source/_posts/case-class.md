---
title: Case Class trong Scala
date: 2018-11-05 10:00:00
categories: 
    - scala
tags: 
    - basic
---

# Case class

Case class giống như class thông thường với một vài từ khóa khác mà chúng ta hay sử dụng. Case class thường được sử dụng cho việc mô hình hóa dữ liệu bất biến. 

## Định nghĩa case class

Một case class tối thiểu cần có từ khóa case class, một định danh, và một danh sách các tham số (có thể trống)
```scala
case class Book(isbn: String) 
val frankenstein = Book("978-0486282114")
```
<!-- more -->

Lưu ý từ khóa new không được sử dụng trong khởi tạo case class Book. Bởi vì case class có một phương thức apply mặc định trong cấu trúc của đối tượng.

Khi bạn tạo mới một case class với tham số, tham số được công khai sử dụng
```scala
case class Message(sender: String, recipient: String, body: String) 
val message1 = Message("guillaume@quebec.ca", "jorge@catalonia.es", "Ça va ?")

println(message1.sender) // prints guillaume@quebec.ca 
message1.sender = "travis@washington.us" // this line does not compile
```

Bạn không thể gán lại giá trị `messager1.sender` bởi vì nó được khai báo val (giá trị không thay đổi). Có thể sử dụng var s trong case class nhưng không được khuyến khích.

## So sánh

Case class được so sánh bằng cấu trúc và không phải bằng tham chiếu :
```scala
case class Message(sender: String, recipient: String, body: String) 
val message2 = Message("jorge@catalonia.es", "guillaume@quebec.ca", "Com va?") 
val message3 = Message("jorge@catalonia.es", "guillaume@quebec.ca", "Com va?") 
val messagesAreTheSame = message2 == message3 // true
```
Mặc dù `message2` và `message3` là 2 đối tượng khác nhau nhưng giá trị của 2 đối tượng bằng nhau. 

## Copying

Bạn có thể tạo mới một bản sao của một case class đơn giản sử dụng phương thức copy. Bạn có thể tùy chọn sửa chữa giá trị .
```scala
case class Message(sender: String, recipient: String, body: String) 
val message4 = Message("julien@bretagne.fr", "travis@washington.us", "Me zo o komz gant ma amezeg") 
val message5 = message4.copy(sender = message4.recipient, recipient = "claire@bourgogne.fr") 
message5.sender // travis@.
message5.recipient // claire@bourgogne.fr 
message5.body // "Me zo o komz gant ma amezeg"
```
Người nhận `message4` được dùng để gửi tiếp tin nhắn 5 nhưng phần thân của tin nhắn 4 được sao chép trực tiếp.


## Nguồn tham khảo:
https://docs.scala-lang.org/tour/case-classes.html

https://www.tutorialspoint.com/scala

#### Biên dịch: Nguyễn Văn Linh