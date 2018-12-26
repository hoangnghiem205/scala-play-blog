---
title: Scala Collections Tips & Tricks (Phần V)
date: 2018-12-10 10:00:00
categories: 
    - scala 
tags: 
    - discovery
---

- Phần I: Sequences (Creation & Length & Equality)
- Phần II: Sequences (Indexing & Existence & Filtering)
- Phần III: Sequences (Sorting & Reduction & Matching & Rewriting)
- Phần IV: Sets
- __Phần V: Options__
- Phần VI: Maps

## 3. Options
### 3.1 Value
#### Không so sánh giá trị của option với `None`
```scala
// Before
option == None
option != None

// After
option.isEmpty
option.isDefined
```

<!-- more -->
#### Không so sánh giá trị option với `Some`
```scala
// Before
option == Some(v)
option != Some(v)

// After
option.contains(v)
!option.contains(v)
```
#### Không dựa vào một phần tử để kiểm tra sự tồn tại của cả mảng
```scala
// Before
option.isInstanceOf[Some[_]]

// After
option.isDefined
```
#### Không sử dụng `match` để kiểm tra sự tồn tại
```scala
// Before
option match {
    case Some(_) => true
    case None => false
}

option match {
    case Some(_) => false
    case None => true
}

// After
option.isDefined
option.isEmpty
```
#### Không phủ định các trong các trường hợp sau
```scala
// Before
!option.isEmpty
!option.isDefined
!option.nonEmpty

// After
seq.isDefined
seq.isEmpty
seq.isEmpty
```
### 3.2 Null
#### Không so sánh giá trị với null để khởi tạo option
```scala
// Before
if (v != null) Some(v) else None

// After
Option(v)
```
#### Không sử dụng `Option` với một hằng số
```scala
// Before
Option("constant")

// After
Some("constant")
```
#### Không để null khi thay thế
```scala
// Before
option.getOrElse(null)

// After
option.orNull
```
### 3.3 Processing
#### Sử dụng `getOrElse`
```scala
// Before
if (option.isDefined) option.get else z

option match {
  case Some(it) => it
  case None => z
}

// After
option.getOrElse(z)
```
#### Sử dụng `orElse`
```scala
if (option1.isDefined) option1 else option2

option1 match {
  case Some(it) => Some(it)
  case None => option2
}

// After
option1.orElse(option2)
```
#### Sử dụng `exists`
```scala
// Before
option.isDefined && p(option.get)

if (option.isDefined) p(option.get) else false

option match {
  case Some(it) => p(it)
  case None => false
}

// After
option.exists(p)
```
#### Sử dụng `forall`
```scala
// Before
option.isEmpty || (option.isDefined && p(option.get))

if (option.isDefined) p(option.get) else true

option match {
  case Some(it) => p(it)
  case None => true
}

// After
option.forall(p)
```
#### Sử dụng `contains`
```scala
// Before
option.isDefined && option.get == x

if (option.isDefined) option.get == x else false

option match {
  case Some(it) => it == x
  case None => false
}

// After
option.contains(x)
```
#### Sử dụng `foreach`
```scala
// Before
if (option.isDefined) f(option.get)

option match {
  case Some(it) => f(it)
  case None =>
}

// After
option.foreach(f)
```
#### Sử dụng `filter`
```scala
// Before
if (option.isDefined && p(option.get)) option else None

option match {
  case Some(it) && p(it) => Some(it)
  case _ => None
}

// After
option.filter(p)
```
#### Sử dụng `map`
```scala
// Before
if (option.isDefined) Some(f(option.get)) else None

option match {
  case Some(it) => Some(f(it))
  case None => None
}

// After
option.map(f)
```
#### Sử dụng `flatMap`
```scala
// Before (f: A => Option[B])
if (option.isDefined) f(option.get) else None

option match {
  case Some(it) => f(it)
  case None => None
}

// After
option.flatMap(f)
```
### 3.4 Rewriting
#### Chuyển đổi `map` với `getOrElse` thành `fold`
```scala
// Before
option.map(f).getOrElse(z)

// After
option.fold(z)(f)
```
#### Sử dụng `exists`
```scala
// Before
option.map(p).getOrElse(false)

// After
option.exists(p)
```
#### Sử dụng `flatten`
```scala
// Before (option: Option[Option[T]])
option.map(_.get)
option.getOrElse(None)

// After
option.flatten
```
#### Không chuyển đổi `option` thành một `sequence ` một cách thủ công
```scala
// Before
option.map(Seq(_)).getOrElse(Seq.empty)
option.getOrElse(Seq.empty) // option: Option[Seq[T]]

// After
option.toSeq
```
… (còn tiếp) …
## Nguồn tham khảo
https://pavelfatin.com/scala-collections-tips-and-tricks/

#### Biên tập bài viết: Nguyễn Văn Linh