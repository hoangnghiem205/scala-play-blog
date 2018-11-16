---
title: Trait trong Scala
date: 2018-11-08 10:00:00
categories: 
    - scala
tags: 
    - basic
---

# Scala trait
Trait bao gồm các định nghĩa về phương thức và trường, và có thể được sử dụng bằng cách kế thừa vào trong class. Không giống như kế thừa class, mỗi một class chỉ được kế thừa 1 class cha, một class có thể kế thừa nhiều trait.

Trait được sử dụng để định nghĩa loại đối tượng bằng cách sử dụng các phương thức được hỗ trợ. Scala cũng cho phép trait thực thi một phần nhưng trait không có tham số.
Một trait được định nghĩa giống như một class được định nghĩa ngoại trừ nó sử dụng từ khóa trait. Theo dõi ví dụ đơn giản về cú pháp của trait.

<!-- more -->

#### Cú pháp:
```scala
trait Equal {
   def isEqual(x: Any): Boolean
   def isNotEqual(x: Any): Boolean = !isEqual(x)
}
```
Trait bao gồm 2 phương thức `isEqual` và `isNotEqual`. Ở đây, chúng tôi không thực hiện `isEqual` khi có một phương thức khác đã thực hiện. Lớp con kế thừa một trait có thể thực hiện hoặc không thực hiện phương thức. Vì vậy một trait rất giống `abstract class` trong java.

Hãy để chúng tôi giả định môt ví dụ của `trait Equal` trong 2 phương thức `isEqual()` và `isNotEqual()`. Trait chứa hàm so sánh chỉ thực hiện 1 phương thức `isEqual()` vì vậy khi người sử dụng định nghĩa `class Point` kế thừa `trait Equal` thực hiện phương thức isEqual() trong class Point cần phải được cung cấp
Ở đây rất cần thiết để biết 2 phương thức quan trọng của Scala, được sử dụng ở ví dụ dưới.

`obj.isInstanceOf [Point]` để kiểm tra loại đối tượng obj và Point là giống hay khác.

`obj.asInstanceOf [Point]` nghĩa là đúc chính xác loại đối tượng obj và trả về cùng một obj như Point.

Thử theo dõi chương trình ví dụ dưới đây để thực hiện Trait.
```scala
 trait Equal {
 def isEqual(x: Any): Boolean
   def isNotEqual(x: Any): Boolean = !isEqual(x)
}

class Point(xc: Int, yc: Int) extends Equal {
   var x: Int = xc
   var y: Int = yc
   
   def isEqual(obj: Any) = obj.isInstanceOf[Point] && obj.asInstanceOf[Point].x == y
}

object Demo {
   def main(args: Array[String]) {
      val p1 = new Point(2, 3)
      val p2 = new Point(2, 4)
      val p3 = new Point(3, 3)
      println(p1.isNotEqual(p2))
      println(p1.isNotEqual(p3))
      println(p1.isNotEqual(2))
   }
}
```
Lưu vào chương trình `Demo.scala`. Các lệnh sau được sử dụng để biên dịch và chạy chương trình này:

#### Command:
```scala
\>scalac Demo.scala
\>scala Demo
```
#### Output
```scala
true
false
true
```
### Value classes và Universal Traits

Value classes là cơ chế mới trong Scala để tránh phân bổ thời gian chạy của các đối tượng. Nó chứa một hàm tạo chính và chính xác 1 tham số val. Nó chỉ chứa các phương thức (def) không có var, val, các lớp lồng nhau, trait hoặc đối tượng. Value classes không thế được kế thừa bởi class khác. Nó có thể khả thi khi kế thừa value class với AnyVal. 

Hãy để chúng tôi làm một ví dụ về value classes Weight, Height,Email,Age, … Đối với tất cả các ví dụ này không cần phải cấp phát bộ nhớ cho ứng dụng.
Một value classes không được phép kế thừa Trait. Để cho phép value classes kế thừa trait, universal traits được giới thiệu và được kế thừa cho bất kỳ value classes nào.

#### Example
```scala
trait Printable extends Any {
   def print(): Unit = println(this)
}

class Wrapper(val underlying: Int) extends AnyVal with Printable
object Demo {
   def main(args: Array[String]) {
      val w = new Wrapper(3)
      w.print() // actually requires instantiating a Wrapper instance
   }
}
```
Lưu vào chương trình `Demo.scala`. Các lệnh sau được sử dụng để biên dịch và chạy chương trình này:

#### Command
```scala
\>scalac Demo.scala
\>scala Demo
```
#### Output
```scala
Wrapper@13
```
### Khi nào sử dụng Trait?

Không có một quy luật chắc chắn, nhưng đây là một ít hướng dẫn để xem xét:
- Nếu hành vi không được tái sử dụng, làm nó trở thành một lớp thực. Nó không phải là hành vi sử dụng sau tất cả.
- Nếu nó được tái sử dụng trong unrelated classes, cho nó là trait. Chỉ trait có thể được kế thừa vào các thành phần khác nhau của lớp.
- Nếu bạn muốn thừa kế từ nó trong java code, sử dụng một abstract class
- Nếu bạn có ý tưởng phân tán nó trong một dạng biên dịch, và bạn chờ đợi các lớp bên ngoài kế thừa từ nó, bạn nên sử dụng abstract class.
- nếu hiêu quả quan trọng, nghiêng về sử dụng class.

## Nguồn tham khảo
https://www.tutorialspoint.com/scala/scala_traits.htm

https://docs.scala-lang.org/overviews/collections/trait-iterable.html


#### Biên tập bài viết: Nguyễn Văn Linh