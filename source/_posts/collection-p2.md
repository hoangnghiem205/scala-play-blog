---
title: Collection trong Scala (phần II - phần cuối)
date: 2018-11-15 10:00:00
categories: 
    - scala
tags: 
    - basic
---
## Functional Combinators

### map
Trả về một danh sách có cùng số phần tử.
```scala
scala> val numbers = List(1, 2, 3, 4)
numbers: List[Int] = List(1, 2, 3, 4)

scala> numbers.map((i: Int) => i * 2)
res0: List[Int] = List(2, 4, 6, 8)
```
<!-- more -->

Hoặc truyền vào một hàm:
```scala
scala> def timesTwo(i: Int): Int = i * 2
timesTwo: (i: Int)Int

scala> numbers.map(timesTwo)
res0: List[Int] = List(2, 4, 6, 8)
```
### foreach
forech giống map nhưng không trả về gì.
```scala
scala> numbers.foreach((i: Int) => i * 2)
```
### filter
Kiểm tra điều kiện của Collection, loại bỏ những phần tử không đáp ứng điều kiện.
```scala
scala> numbers.filter((i: Int) => i % 2 == 0)
res0: List[Int] = List(2, 4)
scala> def isEven(i: Int): Boolean = i % 2 == 0
isEven: (i: Int)Boolean

scala> numbers.filter(isEven)
res2: List[Int] = List(2, 4)
```
### zip
zip tổng hợp 2 list thành 1 list chứa các cặp phần tử.
```scala
scala> List(1, 2, 3).zip(List("a", "b", "c"))
res0: List[(Int, String)] = List((1,a), (2,b), (3,c))
```

### partition
partition chia tách một list .
```scala
scala> val numbers = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
scala> numbers.partition(_ % 2 == 0)
res0: (List[Int], List[Int]) = (List(2, 4, 6, 8, 10),List(1, 3, 5, 7, 9))
```
### find
Trả về phần tử đầu tiên của danh sách thỏa mãn điều kiện.
```scala
scala> numbers.find((i: Int) => i > 5)
res0: Option[Int] = Some(6)
```

### drop & dropWhile
drop xóa i phần tử đầu tiên trong danh sách.
```scala
scala> numbers.drop(5)
res0: List[Int] = List(6, 7, 8, 9, 10)
```
dropWhile loại bỏ những phần tử đầu tiên thỏa mãn điều kiện.
```scala
scala> numbers.dropWhile(_ % 2 != 0)
res0: List[Int] = List(2, 3, 4, 5, 6, 7, 8, 9, 10)
```
### foldLeft

```scala
scala> numbers.foldLeft(0)((m: Int, n: Int) => m + n)
res0: Int = 55
```
number là một `List[Int]` và m hoạt động như một bộ tích lũy.
```scala
scala> numbers.foldLeft(0) { (m: Int, n: Int) => println("m: " + m + " n: " + n); m + n }
m: 0 n: 1
m: 1 n: 2
m: 3 n: 3
m: 6 n: 4
m: 10 n: 5
m: 15 n: 6
m: 21 n: 7
m: 28 n: 8
m: 36 n: 9
m: 45 n: 10
res0: Int = 55
```
### foldRight
Giống foldLeft nhưng chạy ngược lại.
```scala
scala> numbers.foldRight(0) { (m: Int, n: Int) => println("m: " + m + " n: " + n); m + n }
m: 10 n: 0
m: 9 n: 10
m: 8 n: 19
m: 7 n: 27
m: 6 n: 34
m: 5 n: 40
m: 4 n: 45
m: 3 n: 49
m: 2 n: 52
m: 1 n: 54
res0: Int = 55
```
### flatten
```scala
scala> List(List(1, 2), List(3, 4)).flatten
res0: List[Int] = List(1, 2, 3, 4)
```

### flatMap

flatMap hoạt động trên danh sách lồng nhau, sau đó gộp kết quả vào nhau.
```scala
scala> val nestedNumbers = List(List(1, 2), List(3, 4))
nestedNumbers: List[List[Int]] = List(List(1, 2), List(3, 4))

scala> nestedNumbers.flatMap(x => x.map(_ * 2))
res0: List[Int] = List(2, 4, 6, 8)
```
```scala
scala> nestedNumbers.map((x: List[Int]) => x.map(_ * 2)).flatten
res1: List[Int] = List(2, 4, 6, 8)
```
## Generalized functional combinators

Bây giờ chúng ta đã học được các function làm việc với collection. Chúng ta có thể viết một chức năng xử lý collection của riêng mình.

Ex:
```scala
def ourMap(numbers: List[Int], fn: Int => Int): List[Int] = {
  numbers.foldRight(List[Int]()) { (x: Int, xs: List[Int]) =>
    fn(x) :: xs
  }
}

scala> ourMap(numbers, timesTwo(_))
res0: List[Int] = List(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)
```
### Map

Tất cả function đều hoạt động trên Map, Map là một danh sách các cặp key - value.
```scala
scala> val extensions = Map("steve" -> 100, "bob" -> 101, "joe" -> 201)
extensions: scala.collection.immutable.Map[String,Int] = Map((steve,100), (bob,101), (joe,201))
```
Bây giờ lọc ra giá trị value nhỏ hơn 200.
```scala
scala> extensions.filter((namePhone: (String, Int)) => namePhone._2 < 200)
res0: scala.collection.immutable.Map[String,Int] = Map((steve,100), (bob,101))
```
Chúng ta có thể sử dụng `pattern matching` để trích xuất ra giá trị.
```scala
scala> extensions.filter({case (name, extension) => extension < 200})
res0: scala.collection.immutable.Map[String,Int] = Map((steve,100), (bob,101))
```

## Nguồn tham khảo:
https://twitter.github.io/scala_school/collections.html

#### Biên tập bài viết: Nguyễn Văn Linh