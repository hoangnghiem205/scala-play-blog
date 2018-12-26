---
title: Scala Collections Tips & Tricks (Phần III)
date: 2018-12-03 10:00:00
categories: 
    - scala 
tags: 
    - discovery
---
- Phần I: Sequences (Creation & Length & Equality)
- Phần II: Sequences (Indexing & Existence & Filtering)
- __Phần III: Sequences (Sorting & Reduction & Matching & Rewriting)__
- Phần IV: Sets
- Phần V: Options
- Phần VI: Maps

## 1. Sequences
### 1.7 Sorting
#### Không sắp xếp thuộc tính theo cách thủ công
```scala
// Before
seq.sortWith(_.property <  _.property)

// After
seq.sortBy(_.property)
```
<!-- more -->

#### Không sắp xếp `identity` theo cách thủ công
```scala
// Before
seq.sortBy(identity)
seq.sortWith(_ < _)

// After
seq.sorted
```
#### Đảo ngược sắp xếp
```scala
// Before
seq.sorted.reverse
seq.sortBy(_.property).reverse
seq.sortWith(f(_, _)).reverse

// After
seq.sorted(Ordering[T].reverse)
seq.sortBy(_.property)(Ordering[T].reverse)
seq.sortWith(!f(_, _))
```
#### Không sử dụng việc sắp xếp để tìm phần tử nhỏ nhất
```scala
// Before
seq.sorted.head
seq.sortBy(_.property).head

// After
seq.min
seq.minBy(_.property)
```
#### Không sử dụng sắp xếp để tìm phần tử lớn nhất
```scala
// Before
seq.sorted.last
seq.sortBy(_.property).last

// After
seq.max
seq.maxBy(_.property)
```
### 1.8 Reduction
#### Không tính tổng theo cách thủ công
```scala
// Before
seq.reduce(_ + _)
seq.fold(z)(_ + _)

// After
seq.sum
seq.sum + z
```
#### Không tính tích một cách thủ công
```scala
// Before
seq.reduce(_ * _)
seq.fold(z)(_ * _)

// After
seq.product
seq.product * z
```
#### Không tìm kiếm phần tử nhỏ nhất một cách thủ công
```scala
// Before
seq.reduce(_ min _)
seq.fold(z)(_ min _)

// After
seq.min
z min seq.min
```
#### Không tìm kiếm phần tử lớn nhất một cách thủ công
```scala
// Before
seq.reduce(_ max _)
seq.fold(z)(_ max _)

// After
seq.max
z max seq.max
```
#### Sử dụng `forall`
```scala
// Before
seq.foldLeft(true)((x, y) => x && p(y))
!seq.map(p).contains(false)

// After
seq.forall(p)
```
#### Sử dụng `exist`
```scala
// Before
seq.foldLeft(false)((x, y) => x || p(y))
seq.map(p).contains(true)

// After
seq.exists(p)
```
#### Sử dụng `map`
```scala
// Before
seq.foldLeft(Seq.empty)((acc, x) => acc :+ f(x))
seq.foldRight(Seq.empty)((x, acc) => f(x) +: acc)

// After
seq.map(f)
```
#### Sử dụng `filter`
```scala
// Before
seq.foldLeft(Seq.empty)((acc, x) => if (p(x)) acc :+ x else acc)
seq.foldRight(Seq.empty)((x, acc) => if (p(x)) x +: acc else acc)

// After
seq.filter(p)
```
#### Sử dụng `reverse`
```scala
// Before
seq.foldLeft(Seq.empty)((acc, x) => x +: acc)
seq.foldRight(Seq.empty)((x, acc) => acc :+ x)

// After
seq.reverse
```
### 1.9 Matching
Dưới đây là một số mẹo thông dụng có liên quan đến tính năng `pattern matching` và `partial functions`.

#### Sử dụng `partial functions` thay cho `pattern matching`
```scala
// Before
seq.map {
  _ match {
    case P => ??? // x N
  }
}

// After
seq.map {
  case P => ??? // x N
}
```
#### Sử dụng `collect` thay cho `flatMap`
```scala
// Before
seq.flatMap {
  case P => Seq(???) // x N
  case _ => Seq.empty
}

// After
seq.collect {
  case P => ??? // x N
}
```
#### Sử dụng `collect` thay cho `match` khi kết quả là `collection`
```scala
// Before
v match {
  case P => Seq(???) // x N
  case _ => Seq.empty
}

// After
Seq(v) collect {
  case P => ??? // x N
}
```
#### Sử dụng `collectFirst`
```scala
// Before
seq.collect{case P => ???}.headOption

// After
seq.collectFirst{case P => ???}
```
### 1.10 Rewriting

#### Hợp nhất `filter`
```scala
// Before
seq.filter(p1).filter(p2)

// After
seq.filter(x => p1(x) && p2(x))
```
#### Hợp nhất `map`
```scala
// Before
seq.map(f).map(g)

// After
seq.map(f.andThen(g))
```
#### Sắp xếp sau khi lọc
```scala
// Before
seq.sorted.filter(p)

// After
seq.filter(p).sorted
```
#### Không đảo ngược `collection` trước khi gọi `map`
```scala
// Before
seq.reverse.map(f)

// After
seq.reverseMap(f)
```
#### Không đảo ngược `collection` trong trường hợp này
```scala
// Before
seq.reverse.iterator

// After
seq.reverseIterator
```
#### Không chuyển đổi `collection` thành `Set` để tìm các phần tử riêng biệt
```scala
// Before
seq.toSet.toSeq

// After
seq.distinct
```
#### Sử dụng `slice`
```scala
// Before
seq.drop(x).take(y)

// After
seq.slice(x, x + y)
```
#### Sử dụng `splitAt`
```scala
// Before
val seq1 = seq.take(n)
val seq2 = seq.drop(n)

// After
val (seq1, seq2) = seq.splitAt(n)
```
#### Sử dụng `span`
```scala
// Before
val seq1 = seq.takeWhile(p)
val seq2 = seq.dropWhile(p)

// After
val (seq1, seq2) = seq.span(p)
```
#### Sử dụng `partition`
```scala
// Before
val seq1 = seq.filter(p)
val seq2 = seq.filterNot(p)

// After
val (seq1, seq2) = seq.partition(p)
```
#### Sử dụng `takeRight`
```scala
// Before
seq.reverse.take(n).reverse

// After
seq.takeRight(n)
```
#### Sử dụng `flatten`
```scala
// Before (seq: Seq[Seq[T]])
seq.reduce(_ ++ _)
seq.fold(Seq.empty)(_ ++ _)
seq.flatMap(identity)

// After
seq.flatten
```
#### Sử dụng `flatMap`
```scala
// Before (f: A => Seq[B])
seq.map(f).flatten

// After
seq.flatMap(f)
```
#### Không sử dung `map` khi kết quả trả về được bỏ qua
```scala
// Before
seq.map(???) // the result is ignored

// After
seq.foreach(???)
```
#### Không SỬ dụng `unzip` để trích xuất 1 thành phần
```scala
// Before (seq: Seq[(A, B]])
seq.unzip._1

// After
seq.map(_._1)
```
### Không tạo 1 `collection` tạm thời
#### 1. Chuyển đổi `collection` thành 1 gía trị duy nhất
```scala
// Before
seq.map(f).flatMap(g).filter(p).reduce(???)

// After
seq.view.map(f).flatMap(g).filter(p).reduce(???)
```
#### 2. Chuyển đổi tạo ra một `collection` từ cùng một lớp
```scala
// Before
seq.map(f).flatMap(g).filter(p)

// After
seq.view.map(f).flatMap(g).filter(p).force
```
Nếu phép chuyển đổi trung gian duy nhất là `filter`, bạn có thể xem xét sử dụng phương thức withFilter như một phương án thay thế:
```scala
seq.withFilter(p).map(f)
```
#### 3. Chuyển đổi tạo ra một `collection` của các lớp khác nhau
```scala
// Before
seq.map(f).flatMap(g).filter(p).toList

// After
seq.view.map(f).flatMap(g).filter(p).toList
```
Có một cách khác để xử lý trường hợp này dựa trên `breakOut`:
```scala
seq.map(f)(collection.breakOut): List[T]
```
Biểu thức như vậy có chức năng tương đương với việc sử dụng một khung nhìn, tuy nhiên cách tiếp cận này:
- Cần rõ ràng
- Bị giới hạn trong một chuyển đổi duy nhất.
- Khá phức tạp

#### Sử dụng toán tử gán để gán lại một chuỗi
```scala
// Before
seq = seq :+ x
seq = x +: seq
seq1 = seq1 ++ seq2
seq1 = seq2 ++ seq1

// After
seq :+= x
seq +:= x
seq1 ++= seq2
seq1 ++:= seq2
```
Ngoài ra còn một cú pháp đặc biệt:
```scala
// Before
list = x :: list
list1 = list2 ::: list1

stream = x #:: stream
stream1 = stream2 #::: stream1

// After
list ::= x
list1 :::= list2

stream #::= x
stream1 #:::= stream2
```
#### Không chuyển đổi `collection` theo cách thủ công
```scala
// Before
seq.foldLeft(Set.empty)(_ + _)
seq.foldRight(List.empty)(_ :: _)

// After
seq.toSet
seq.toList
```
#### Hãy cẩn thận với `toSeq` về các `collection` không nghiêm ngặt
```scala
// Before (seq: TraversableOnce[T])
seq.toSeq

// After
seq.toStream
seq.toVector
```
Bởi vì Seq (...) tạo ra một bộ sưu tập nghiêm ngặt (cụ thể là Vector), chúng ta có thể phải sử dụng `toSeq` để chuyển đổi một thực thể không nghiêm ngặt (như Stream, Iterator hoặc "view") thành một `collection` nghiêm ngặt.

Dưới đây là một ví dụ điển hình về lỗ hổng
```scala
val source = Source.fromFile("lines.txt")
val lines = source.getLines.toSeq
source.close()
lines.foreach(println)
```
#### Không chuyển đổi chuỗi theo cách thủ công
```scala
// Before (seq: Seq[String])
seq.reduce(_ + _)
seq.reduce(_ + separator + _)
seq.fold(prefix)(_ + _)
seq.map(_.toString).reduce(_ + _) // seq: Seq[T]
seq.foldLeft(new StringBuilder())(_ append _)

// After
seq.mkString
seq.mkString(prefix, separator, "")
```
… (còn tiếp) …
## Nguồn tham khảo
https://pavelfatin.com/scala-collections-tips-and-tricks/

#### Biên tập bài viết: Nguyễn Văn Linh