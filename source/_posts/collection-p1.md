---
title: Collection trong Scala (phần I)
date: 2018-11-12 10:00:00
categories: 
    - scala
tags: 
    - basic
---
Đây là bài viết khá hay về  `collection` trong Scala. Vì bài viết tương đối dài nên mình sẽ chia làm hai bài blog để các bạn tiện theo dõi. Chủ đề chúng ta sẽ follow trong hai bài blog đó là:
- __Basic Data Structures__: Collection trong Scala (phần I)
- __Functional Combinators__: Collection trong Scala (phần II - phần cuối)

## Basic Data Structures

Scala cung cấp một số collection rất hữu dụng.
### Array
Array có trật tự, có thể lưu các bản sao của các phần tử và có thể thay đôi được.
```scala
scala> val numbers = Array(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
numbers: Array[Int] = Array(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)

scala> numbers(3) = 10
```
<!-- more -->

### Lists

List có trật tự, có thể lưu các bản sao của các phần tử  và có thể thay đổi được.

```scala
scala> val numbers = List(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
numbers: List[Int] = List(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)

scala> numbers(3) = 10
<console>:9: error: value update is not a member of List[Int]
              numbers(3) = 10
```
### Sets

Set không có trật tự và không thể lưu các bản sao của các phần tử .

```scala
scala> val numbers = Set(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
numbers: scala.collection.immutable.Set[Int] = Set(5, 1, 2, 3, 4)
```
### Tuple
Tuple nhóm các phần tử lại mà không sử dụng một Class.

```scala
scala> val hostPort = ("localhost", 80)
hostPort: (String, Int) = (localhost, 80)
```
Không giống các case class, chúng không có tên truy nhập, thay vào đó chúng được truy nhập theo vị trí.
```scala
scala> hostPort._1
res0: String = localhost

scala> hostPort._2
res1: Int = 80
```
Pattern matching với tuple.
```scala
hostPort match {
  case ("localhost", port) => ...
  case (host, port) => ...
}
```
### Map
Nó có thể chứa các kiểu dữ liệu cơ bản.

```scala
Map(1 -> 2)
Map("foo" -> "bar")
```
Map được sử dụng cú pháp đối số. ``Map(1 -> "one", 2 -> "two")`` giống như ``Map((1, "one"), (2, "two"))``
với giá trị đầu là key và giá trị tiếp theo là value.

Map có thể chứa nhiều kiểu giá trị
```scala
Map(1 -> Map("foo" -> "bar"))
Map("timesTwo" -> { timesTwo(_) })
```
### Option
Option là một container có thể có dữ liệu hoặc không có gì.

Interface cơ bản của Option giống như:
```scala
trait Option[T] {
  def isDefined: Boolean
  def get: T
  def getOrElse(t: T): T
}
```
Option có 2 lớp tùy chọn là: `Some[T]` hoặc `None`

Hãy xem một ví dụ về cách sử dụng Option:

```scala
Map.get uses Option for its return type. Option tells you that the method might not return what you’re asking for.

scala> val numbers = Map("one" -> 1, "two" -> 2)
numbers: scala.collection.immutable.Map[java.lang.String,Int] = Map(one -> 1, two -> 2)

scala> numbers.get("two")
res0: Option[Int] = Some(2)

scala> numbers.get("three")
res1: Option[Int] = None
```
Dữ liệu trên sẽ bị lỗi. Làm sao để làm việc với nó?

Sử dụng `isDefined`
```scala
// We want to multiply the number by two, otherwise return 0.
val result = if (res1.isDefined) {
  res1.get * 2
} else {
  0
}
```
Bạn nên sử dụng `getOrElse`, vì nó cho phép bạn dễ dàng gán giá trị mặc định.
```scala
val result = res1.getOrElse(0) * 2
```
Pattern matching với Option:
```scala
val result = res1 match {
  case Some(n) => n * 2
  case None => 0
}
```
... (còn tiếp) ...

## Nguồn tham khảo:
https://twitter.github.io/scala_school/collections.html

#### Biên tập: Nguyễn Văn Linh
