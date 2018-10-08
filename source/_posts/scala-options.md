---
title: Scala – Options
date: 2018-10-08 17:07:20
tags:
---

&nbsp;&nbsp;&nbsp;&nbsp; Scala Option [ T ] là một vùng chứa cho 0 hoặc một phần tử của một kiểu đã cho. Một Option[ T ] có thể là đối tượng __Some[ T ]__  hoặc __None__, biểu thị giá trị bị thiếu. Ví dụ, phương thức get của Map trong Scala tạo ra Some (value) nếu một giá trị tương ứng với một khóa đã cho đã được tìm thấy, hoặc __None__ nếu khóa đã cho không được định nghĩa trong Map.

Kiểu Option được dùng thường xuyên trong chương trình scala và bạn có thể so sánh nó với giá trị __null__ có sẵn trong Java để chỉ không có giá trị. Ví dụ , phương thức get trong java.util.HashMap  trả về  giá trị được lưu trong HashMap ,hoặc null nếu giá trị không được  tìm thấy.

Giả sử chúng ta có một phương thức lấy một bản ghi từ cơ sở dữ liệu dựa trên khóa chính.

def findPerson(key: Int): Option[Person]

Phương thức này sẽ trả về Some[Person]  nếu bản ghi được tìm thấy hoặc None nếu bản ghi không được tìm thấy. Chúng ta cùng theo dõi chương trình sau đây.

### Ví dụ
```scala
object Demo {
    def main(args: Array[String]) {
        val capitals = Map("France" -> "Paris", "Japan" -> "Tokyo")
        println("capitals.get( \"France\" ) : " +  capitals.get( "France" ))
        println("capitals.get( \"India\" ) : " +  capitals.get( "India" ))
    }
}
```

Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.

## Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```
## Kết quả
```
capitals.get( "France" ) : Some(Paris)
capitals.get( "India" ) : None
```

Cách phổ biến nhất để lấy các giá trị tùy ý sang một bên là thông qua a pattern match. Ví dụ, hãy thử chương trình sau.

### Ví dụ
```scala
object Demo {
   def main(args: Array[String]) {
      val capitals = Map("France" -> "Paris", "Japan" -> "Tokyo")
      println("show(capitals.get( \"Japan\")) : " + show(capitals.get( "Japan")) )
      println("show(capitals.get( \"India\")) : " + show(capitals.get( "India")) )
   }
   def show(x: Option[String]) = x match {
      case Some(s) => s
      case None => "?"
   }
}
```

Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.

## Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```
## Kết quả
```
show(capitals.get( "Japan")) : Tokyo
show(capitals.get( "India")) : ?
```

# <p style="color:red"> Dùng phương thức getOrElse() </p>
&nbsp;&nbsp;&nbsp;&nbsp; Sau đây là chương trình ví dụ để chỉ cách sử dụng phương thức getOrElse () để truy cập một giá trị hoặc một giá trị mặc định khi không có giá trị nào.

### Ví dụ

```scala
object Demo {
   def main(args: Array[String]) {
      val a:Option[Int] = Some(5)
      val b:Option[Int] = None 
      println("a.getOrElse(0): " + a.getOrElse(0) )
      println("b.getOrElse(10): " + b.getOrElse(10) )
   }
}
```

Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.

## Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```
## Kết quả
```
a.getOrElse(0): 5
b.getOrElse(10): 10
```

# <p style="color:red"> Dùng phương thức isEmpty() </p>

&nbsp;&nbsp;&nbsp;&nbsp; Sau dây là chương trình ví dụ để chỉ cách dùng phương thức isEmpty() để kiểm tra Option  là None hay không

### Ví dụ

```scala
object Demo {
   def main(args: Array[String]) {
      val a:Option[Int] = Some(5)
      val b:Option[Int] = None 
      println("a.isEmpty: " + a.isEmpty )
      println("b.isEmpty: " + b.isEmpty )
   }
}
```

Lưu chương trình trên vào file __Demo.scala.__ Lệnh sau đây được dùng để biên dịch và thực thi chương trình Demo.

## Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```
## Kết quả
```
a.isEmpty: false
b.isEmpty: true
```

# <p style="color:red"> Những phương thức Option trong Scala </p>

&nbsp;&nbsp;&nbsp;&nbsp; Sau đây là các phương thức quan trọng mà bạn có thể sử dụng khi lựa chọn Options. Để biết danh sách đầy đủ các phương pháp có sẵn, vui lòng kiểm tra tài liệu chính thức của Scala.

| Sr.No | <p style="text-align:center"> Phương thức với mô tả </p>|
|----------|-------------------|
| 1 |<p> __def get: A__</p> Trả về giá trị của option |
| 2 |<p> __def isEmpty: Boolean__</p> Trả về true nếu option là None, ngược lại thì false |
| 3 |<p> __def productArity: Int__</p> Kích thước của kết quả này. Đối với một kết quả A(x_1, ..., x_k) trả về k. |
| 4 |<p> __def productElement(n: Int): Any__</p> Phần tử thứ n 			của kết quả này , dựa trên 0. Nói cách khác, đối với 			một kết quả   A(x_1, ..., x_k), trả về x_(n + 1) trong đó 0 < n < k. |
| 5 |<p> __def exists(p: (A) => Boolean): Boolean__</p> Trả về true nếu option này khác rỗng và hàm p trả về true khi áp dụng cho giá trị của Option này. Nếu không, trả về false. |
| 6 |<p> __def filter(p: (A) => Boolean): Option[A]__</p> Trả 			về Option này nếu nó khác rỗng, và áp dụng hàm p cho giá trị của Option này trả về true. Nếu không, trả về None. |
| 7 |<p> __def filterNot(p: (A) => Boolean): Option[A]__</p> Trả về Option này nếu nó khác rỗng, và áp dụng hàm p cho giá trị của Option này trả về false. Nếu không, trả về None. |
| 8 |<p> __def flatMap[B](f: (A) => Option[B]): Option[B]__</p> Trả về kết quả của áp dụng hàm f cho giá trị của Option nếu Option này khác rỗng. Trả về None nếu Option này rỗng. |
| 9 |<p> __def foreach[U](f: (A) => U): Unit__</p> Áp dụng hàm f cho giá trị của option ,nếu nó khác rỗng. Ngược lại, không làm gì cả |
| 10 |<p> __def getOrElse[B >: A](default: => B): B__</p> Trả về giá trị của option nếu option này khác rỗng, ngược lại trả về kết quả của đánh giá default mặc định. |
| 11 |<p> __def isDefined: Boolean__</p> Trả về true nếu option này là một thể hiện của kiểu Some , ngược lại false. |
| 12 |<p> __def iterator: Iterator[A]__</p> Trả về một trình lặp singleton trả về giá trị của Option nếu nó khác rỗng, hoặc một trình lặp rỗng nếu option rỗng. |
| 13 |<p> __def map[B](f: (A) => B): Option[B]__</p> Trả về một Some chứa kết quả của áp dụng hàm f cho giá trị của Option này nếu Option này khác rỗng. Nếu không, trả về None. |
| 14 |<p> __def orElse[B >: A](alternative: => Option[B]): Option[B]__</p> Trả về Option này nếu nó khác rỗng , không thì trả về  kết quả của đánh giá  thay thế alternative. |
| 15 |<p> __def orNull__</p> Trả về giá trị của option nếu nó khác rỗng, hoặc trả về null nếu nó rỗng. |







