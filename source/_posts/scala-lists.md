---
title: Giới thiệu về List trong Scala
date: 2018-10-16 09:11:35
categories: scala
tags: basic
---

&nbsp;&nbsp;&nbsp;&nbsp;`List` trong scala gần tương tự như mảng, vì mọi phần tử đều cùng kiểu dữ liệu nhưng lại có hai sự khác biệt quan trọng:
- Đầu tiên, `List` không thể thay đổi, có nghĩa những phần tử trong một List không thể thay đổi được bằng phép gán.

<!-- more -->

- Thứ hai, `List` đại diện cho một danh sách liên kết các phần tử, trong khi đó các phần tử của array(mảng) đước xếp cạnh, liên tiếp với nhau.

Kiểu của một List được viết duới dạng : `List[T]`. 

Bạn hãy xem thử ví dụ dưới đây, một vài List được định nghĩa cho các kiểu dữ liệu khác nhau.

```scala
// List of Strings
val fruit: List[String] = List("apples", "oranges", "pears")

// List of Integers
val nums: List[Int] = List(1, 2, 3, 4)

// Empty List.
val empty: List[Nothing] = List()

// Two dimensional list
val dim: List[List[Int]] =
   List(
      List(1, 0, 0),
      List(0, 1, 0),
      List(0, 0, 1)
   )
```

Tất cả List được định nghĩa với hai khối cơ bản là :: và Nil ở cuối. Nil cũng biểu thị một danh sách trống. Tất cả List trên được định nghĩa như sau:

```scala
// List of Strings
val fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
// List of Integers
val nums = 1 :: (2 :: (3 :: (4 :: Nil)))
// Empty List.
val empty = Nil
// Two dimensional list
val dim =   (1 :: (0 :: (0 :: Nil))) ::
            (0 :: (1 :: (0 :: Nil))) ::
            (0 :: (0 :: (1 :: Nil))) :: Nil
```
Vừa rồi là một số giới thiệu sơ lược của mình về List trong Scala. Tiếp sau đây chúng ta sẽ đi tìm hiểu sâu hơn về một số cách thao tác và phương thức sử dụng trong List

## Thao tác cơ bản với List

Tất cả thao tác trên List được thể hiện theo ba phương thức sau đây:

|<p style="width:50px"> No </p> |                 <p style="text-align:center;height:50%"> Phương thức và Mô tả </p>                       |
|-----|-------------------------------------------------------------|
|  1  |  __head:__ Phương thức này trả về phần tử đầu tiên của list. |
|  2  |  __tail:__ Phương thức này trả về 1 danh sách gồm tất cả phần tử ngoại trừ phần tử đầu tiên.|
|  3  |  __isEmpty:__ Phương thức này trả về  true nếu list rỗng, và ngược lại trả về false.|

Ví dụ sau cho ta thấy cách dùng các phương thức trên.

_Ví dụ:_

```scala
 object Demo {
  def main(args: Array[String]) {
     val fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
     val nums = Nil
     println( "Head of fruit : " + fruit.head )
     println( "Tail of fruit : " + fruit.tail )
     println( "Check if fruit is empty : " + fruit.isEmpty )
     println( "Check if nums is empty : " + nums.isEmpty )
  }
}
```

Lưu chương trình trên trong tập tin __Demo.scala.__    Lệnh sau được dùng để biên dịch và thực thi chương trình này.

### Cài đặt
```scala
\>scalac Demo.scala
\>scala Demo
```

### Kết quả

```
Head of fruit : apples
Tail of fruit : List(oranges, pears)
Check if fruit is empty : false
Check if nums is empty : true
```

## Ghép nối các List
Bạn có thể dùng hoặc phép toán __:::__ hoặc phương thức __List.:::()__ hoặc __List.concat()__ để thêm hai hoặc nhiều List trở lên. Vui lòng khám phá ví dụ dưới đây

### Ví dụ

```scala
object Demo {
    def main(args: Array[String]) {
         val fruit1 = "apples" :: ("oranges" :: ("pears" :: Nil))
         val fruit2 = "mangoes" :: ("banana" :: Nil)
        // dùng 2 hay nhiều list với toán tử :::
         var fruit = fruit1 ::: fruit2
         println( "fruit1 ::: fruit2 : " + fruit )
          // dùng 2 list với phương thức Set.:::()
         fruit = fruit1.:::(fruit2)
         println( "fruit1.:::(fruit2) : " + fruit )
         // truyền hai hay nhiều list làm tham số
         fruit = List.concat(fruit1, fruit2)
         println( "List.concat(fruit1, fruit2) : " + fruit  )
    }
}
```

Lưu chương trình trên trong tập tin __Demo.scala.__ Những lệnh sau được dùng để biên dịch và thực thi chương trình này.

### Cài đặt

```scala
\>scalac Demo.scala
\>scala Demo
```

### Kết quả

```
fruit1 ::: fruit2 : List(apples, oranges, pears, mangoes, banana)
fruit1.:::(fruit2) : List(mangoes, banana, apples, oranges, pears)
List.concat(fruit1, fruit2) : List(apples, oranges, pears, mangoes, banana)
```

## Tạo List đồng nhất

Bạn có thể dùng phương thức List.fill() để tạo một 0 hoặc nhiều bản sao của các phần tử . Hãy thử chương trình ví dụ sau đây.

### Ví dụ:

```scala
object Demo {
   def main(args: Array[String]) {
      val fruit = List.fill(3)("apples") // Repeats apples three times.
      println( "fruit : " + fruit  )

      val num = List.fill(10)(2)         // Repeats 2, 10 times.
      println( "num : " + num  )
   }
}
```

Lưu chương trình trên trong tập tin __Demo.scala.__ Các Lệnh sau được dùng để biên dịch và thực thi chương trình này.

### Cài đặt:
```scala
\>scalac Demo.scala
\>scala Demo
```

### Kết quả:
```
fruit : List(apples, apples, apples)
num : List(2, 2, 2, 2, 2, 2, 2, 2, 2, 2)
```

## Tạo bảng một hàm

Bạn có  thể dùng một hàm cùng với phương thức  __List.tabulate()__  để tác dụng lên tất cả phần tử của List trước khi tạo bảng List. Tham số của hàm này giống như phương thức List.fill:  Tham số thứ nhất cho biết kích thước List cần tạo, và tham số thứ hai mô tả các phần tử của List. Sự khác biệt duy nhất là thay vì phần tử  cố định , chúng được tính từ một hàm.

Thử chương trình ví dụ sau đây
### Ví dụ:

```scala
object Demo {
    def main(args: Array[String]) {
         // Creates 5 elements using the given function.
         val squares = List.tabulate(6)(n => n * n)
         println( "squares : " + squares  )
         val mul = List.tabulate( 4,5 )( _ * _ )      
         println( "mul : " + mul  )
    }
}
```

Lưu chương trình trên trong tập tin __Demo.scala.__ Các Lệnh sau được dùng để biên dịch và thực thi chương trình này.

### Cài đặt:
```scala
\>scalac Demo.scala
\>scala Demo
```

### Kết quả
```
squares : List(0, 1, 4, 9, 16, 25)
mul : List(List(0, 0, 0, 0, 0), List(0, 1, 2, 3, 4), 
  List(0, 2, 4, 6, 8), List(0, 3, 6, 9, 12))
```

## Đảo ngược thứ tự phần tử trong List

Bạn có thể dùng phương thức __List.reverse__ để đảo ngược tất cả phần tử trong danh sách. Ví dụ sau đây cho thấy cách dùng nó.

### Ví dụ:
```scala
object Demo {
     def main(args: Array[String]) {
     val fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
     println( "Before reverse fruit : " + fruit )
     println( "After reverse fruit : " + fruit.reverse )
  }
}
```

Lưu chương trình trên trong tập tin __Demo.scala.__ Các Lệnh sau được dùng để biên dịch và thực thi chương trình này.

### Cài đặt:
```scala
\>scalac Demo.scala
\>scala Demo
```

### Kết quả:
```
Before reverse fruit : List(apples, oranges, pears)
After reverse fruit : List(pears, oranges, apples)
```
## Các phương thức của List trong scala

Sau đây là những phương thức quan trọng khi bạn dùng khi thao tác với các List. Đối với một danh sách hoàn chỉnh các phương thức có sẵn, hãy kiểm tra tài liệu chính thức của Scala.

|Sr.No 		 |<p style="text-align:center">Phương thức và mô tả</p>|
|----------|-----------------------------------------------------|
|1|<p>__scala def +(elem: A): List[A]__</p> Thêm một phần tử vào đầu list |
|2|<p>__def :::(prefix: List[A]): List[A]__</p> Thêm những phần tử của list được truyền, vào trước list ban đầu |
|3|<p>__def ::(x: A): List[A]__</p> Thêm một phần tử x vào đầu của list |
|4|<p>__def addString(b: StringBuilder): StringBuilder__</p> Gán tất cả phần tử của list vào chuỗi StringBuilder |
|5|<p>__def addString(b: StringBuilder, sep: String): StringBuilder__</p> Gán tất cả phần tử của list vào chuỗi StringBuilder b và ngăn cách lần lượt bởi chuỗi sep |
|6|<p>__def apply(n: Int): A__</p> Chỉ định và trả về một phần tử bởi chỉ số của nó trong list |
|7|<p>__def contains(elem: Any): Boolean__</p> Kiểm tra  giá trị được truyền có là một phần tử  trong list không. |
|8|<p>__def copyToArray(xs: Array[A], start: Int, len: Int): Unit__</p>	Sao 			chép tất cả phần tử của list sang 1 mảng. Tham số  cần truyền thêm là vị trí bắt đầu được sao chép sang trong mảng, và độ dài của list. |
|9|<p>__def distinct: List[A]__</p> Trả về một list mới từ list đã cho mà không chứa bất kỳ phần tử trùng nào. |
|10|<p>__def drop(n: Int): List[A]__</p> Trả về list  chứa tất cả phần tử ngoại trừ  n phần tử đầu tiên. |
|11|<p>__def dropRight(n: Int): List[A]__</p> Trả về list chứa tất cả phần tử ngoại trừ  n phần tử cuối cùng. |
|12|<p>__def endsWith[B]\(that: Seq[B]): Boolean__</p> Kiểm tra list có kết thúc với danh sách Seq[] truyền vào hay không. |
|13|<p>__def equals(that: Any): Boolean__</p> Phương thức tương tự với các danh sách tùy ý. So sánh danh sách này với một số đối tượng khác. |
|14|<p>__def filter(p: (A) => Boolean): List[A]__</p> Trả về tất cả phần tử trong list thỏa mãn vị từ. |
|15|<p>__def foreach(f: (A) => Unit): Unit__</p> Áp dụng hàm f cho tất cả các phần tử của list. |
|16|<p>__def head: A__</p> Chỉ định phần tử đầu tử đầu tiên của list. |
|17|<p>__def indexOf(elem: A, from: Int): Int__</p> Tìm vị trí xuất hiện đầu tiên của giá trị A trong list ,kể từ vị trí from trở đi. |
|18|<p>__def init: List[A]__</p> Trả về list chứa tất cả phần tử ngoại trừ phần tử  cuối cùng. |
|19|<p>__def isEmpty: Boolean__</p> Kiểm tra xem list có rỗng không |
|20|<p>__def iterator: Iterator[A]__</p> Tạo một trình lặp mới trên tất cả phần tử có trong đối tượng iterable có thể lặp |
|21|<p>__def last: A__</p> Trả về phần tử cuối cùng |
|22|<p>__def lastIndexOf(elem: A, end: Int): Int__</p> Tìm vị trí xuất hiện cuối cùng của một số giá trị trong list ,trước hoặc tại vị trí end . |
|23|<p>__def length: Int__</p> Trả về độ dài của list. |
|24|<p>__def map[B]\(f: (A) => B): List[B]__</p> Trả về một list mới bằng việc áp dụng hàm f cho tất cả phần tử của list. |
|25|<p>__def max: A__</p> Tìm phần tử lớn nhất. |
|26|<p>__def min: A__</p> Tìm phần tử nhỏ nhất. |
|27|<p>__def mkString: String__</p> Hiển thị ra tất cả phần tử của list dưới dạng một chuỗi. |
|28|<p>__def mkString(sep: String): String__</p> Hiển thị ra tất cả phần tử của list trong một chuỗi dùng 1 chuỗi ngăn cách sep |
|29|<p>__def reverse: List[A]__</p> Trả về một list mới với các phần tử được đảo ngược. |
|30|<p>__def sorted[B >: A]: List[A__</p> Sắp xếp list theo một thứ tự. |
|31|<p>__def startsWith[B]\(that: Seq[B], offset: Int): Boolean__</p> Kiểm tra xem list có chứa danh sách truyền vào tại một vị trí offset không. |
|32|<p>__def sum: A__</p> Tính tổng tất cả phần tử của tập hợp. |
|33|<p>__def tail: List[A]__</p> Trả về tất cả phần tử ngoại trừ phần tử đầu tiên. |
|34|<p>__def take(n: Int): List[A]__</p> Trả về  n phần tử đầu tiên. |
|35|<p>__def takeRight(n: Int): List[A]__</p> Trả về  n phần tử cuối cùng. |
|36|<p>__def toArray: Array[A]__</p> Ép kiểu list sang kiểu mảng array. |
|37|<p>__def toBuffer[B >: A]: Buffer[B]__</p> Ép kiểu list sang một bộ đệm có thể thay đổi. |
|38|<p>__def toMap[T, U]: Map[T, U]__</p> Chuyển đổi list sang một map |
|39|<p>__def toSeq: Seq[A]__</p> Chuyển đổi list sang một danh sách sequence. |
|40|<p>__def toSet[B >: A]: Set[B]__</p> Chuyển đổi list sang một Set. |
|41|<p>__def toString(): String__</p> Chuyển đổi list sang một chuỗi. |

## Nguồn tham khảo
https://www.tutorialspoint.com/scala/scala_lists.htm