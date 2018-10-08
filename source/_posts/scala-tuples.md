---
title: Giới thiệu về Tuples trong Scala
date: 2018-10-08 17:37:14
categories: 
    - scala
tags: 
    - basic
---

&nbsp;&nbsp;&nbsp;&nbsp; Trong Scala, Tuple dùng để gộp một số lượng phần tử cố định cùng với nhau. Không như array (mảng) và list (danh sách), một tuple có thể chứa object (đối tượng) với các kiểu dữ liệu khác nhau và chúng không thay đổi được giá trị.

Sau đây là ví dụ về tuple lưu kiểu nguyên và kiểu chuỗi. Đây là một cách khai báo ngắn gọn:

```scala
val t = (1, "hello")
```

Cách khai báo cụ thể sẽ giống như sau:
```scala
val t = new Tuple2(1, "hello")
```

Kiểu thực sự của tuple phụ thuộc vào số lượng và các phần tử và kiểu của các phần tử đó.
Do đó kiểu của (99, "Luftballons") là Tuple2[Int, String]. Kiểu của ('u', 'r', "the", 1, 4, "me") là Tuple6[Char, Char, String, Int, Int, String]. 

Tuples có kiểu Tuple1 ,Tuple2, Tuple3... .Hiện tại tuples có thể chứa tối đa 22 phần tử ,nếu muốn nhiều hơn ,bạn có thể sử dụng Collection khác ngoài tuple. Đối với mỗi kiểu TupleN , trong đó 1<= N <= 22 Scala định nghĩa một số phương thức truy cập phần tử với đinh nghĩa sau-

```scala
val t = (4,3,2,1)
```

Để truy cập các phần tử của tuple t, bạn có thể dùng phương thức t._1 để truy cập phần tử đầu tiên , t._2 để truy cập phần tử thứ hai, và tương tự . Ví dụ, biểu thức sau đây tính tổng các phần tử của tuple t.

```scala
val sum = t._1 + t._2 + t._3 + t._4
```

Bạn có thể dùng Tuple để viết một phương thức lấy kiểu List[Double] làm tham số và trả về biến biến count , biến tổng sum , và tổng bình phương ,được trả về dưới kiểu Tuple 3 phần tử , kiểu Tuple3[Int, Double, Double] .Chúng cũng hữu ích khi truyền danh sách các giá trị dữ liệu dưới dạng thông điệp giữa các tác nhân trong lập trình đồng thời.

Thử chương trình ví dụ sau:

### Ví dụ:
```scala
object Demo {
   def main(args: Array[String]) {
      val t = (4,3,2,1)
      val sum = t._1 + t._2 + t._3 + t._4
      println( "Sum of elements: " + sum )
   }
}
```

Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.

 ### Cài đặt:
```scala
\>scalac Demo.scala
\>scala Demo
```

 ### Kết quả:
```
Sum of elements: 10
```
Phần trên là những giới thiệu qua của mình về Tuple trong Scala. Bây giờ chúng ta sẽ đi tìm hiểu một vài phương thức thường xuyên được sử dụng trong Tuple.
# <p style="color:red"> Phép lặp trên Tuple </p>
Bạn có thể dùng phương thức __Tuple.productIterator()__ để lặp qua hết tất cả các phần tử của Tuple.
Thử ví dụ sau để thực hiện phép lặp trên tuples.
 ### Ví dụ:
```scala
object Demo {
   def main(args: Array[String]) {
      val t = (4,3,2,1)
      t.productIterator.foreach{ i =>println("Value = " + i )}
   }
}
```

Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.

 ### Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```
 ### Kết quả
```
Value = 4
Value = 3
Value = 2
Value = 1
```
# <p style="color:red"> Chuyển đổi thành chuỗi </p>
Bạn có thể sử dụng phương thức __Tuple.toString ()__ để nối tất cả các phần tử của tuple sang kiểu chuỗi. Hãy thử chương trình ví dụ sau để chuyển đổi thành Chuỗi.
### Ví dụ
```scala
object Demo {
   def main(args: Array[String]) {
      val t = new Tuple3(1, "hello", Console)
      println("Concatenated String: " + t.toString() )
}}
```
Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.
 ### Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```
 ### Kết quả
```
Concatenated String: (1,hello,scala.Console$@281acd47)
```
# <p style="color:red"> Đảo các phần tử  trong Tuple</p>
Bạn có thể sử dụng phương thức __Tuple.swap__ để hoán đổi các phần tử của Tuple2. 
Hãy thử chương trình ví dụ sau để hoán đổi các phần tử.
### Ví dụ
```scala
object Demo {
   def main(args: Array[String]) {
      val t = new Tuple2("Scala", "hello")
      println("Swapped Tuple: " + t.swap )
   }
}
```
Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.
 ### Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```
 ### Kết quả
```
Swapped tuple: (hello,Scala)
```

## Nguồn tham khảo
https://www.tutorialspoint.com/scala/scala_tuples.htm
 


