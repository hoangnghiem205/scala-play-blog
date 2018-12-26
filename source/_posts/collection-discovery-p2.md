---
title: Scala Collections Tips & Tricks (Phần II)
date: 2018-11-29 10:00:00
categories: 
    - scala 
tags: 
    - discovery
---
- Phần I: Sequences (Creation & Length & Equality)
- __Phần II: Sequences (Indexing & Existence & Filtering)__
- Phần III: Sequences (Sorting & Reduction & Matching & Rewriting)
- Phần IV: Sets
- Phần V: Options
- Phần VI: Maps

## 1. Sequences
### 1.4 Indexing
#### Không sử dụng chỉ số để lấy phần tử đầu tiên
```scala
// Before
seq(0)

// After
seq.head
```
<!-- more -->

#### Không truy xuất phần tử cuối cùng theo chỉ số
```scala
// Before
seq(seq.length - 1)

// After
seq.last
```
#### Không kiểm tra giới hạn chỉ số một cách rõ ràng
```scala
// Before
if (i < seq.length) Some(seq(i)) else None

// After
seq.lift(i)
```
#### Không mô phỏng theo `headOption`
```scala
// Before
if (seq.nonEmpty) Some(seq.head) else None
seq.lift(0)

// After
seq.headOption
```
#### Không mô phỏng theo `lastOption`
```scala
// Before
if (seq.nonEmpty) Some(seq.last) else None
seq.lift(seq.length - 1)

// After
seq.lastOption
```
#### Cẩn thận khi sử dụng `indexOf` và `lastIndexOf`
```scala
// Before
Seq(1, 2, 3).indexOf("1") // compilable
Seq(1, 2, 3).lastIndexOf("2") // compilable

//  After
Seq(1, 2, 3).indexOf(1)
Seq(1, 2, 3).lastIndexOf(2)
```
#### Không xây dựng phạm vi chỉ số theo cách thủ công
```scala
// Before
Range(0, seq.length)

// After
seq.indices
```
#### Không nén collection bằng chỉ số của nó theo cách thủ công
```scala
// Before
seq.zip(seq.indices)

// After
seq.zipWithIndex
```
#### Sử dụng `IndexedSeq` như một gía trị hàm
```scala
// Before (seq: IndexedSeq[T])
Seq(1, 2, 3).map(seq(_))

// After
Seq(1, 2, 3).map(seq)
```
### 1.5 Existence

#### Cách kiểm tra sự tồn tại của một phần tử
```scala
// Before
seq.exists(_ == x)

//  After
seq.contains(x)
```
#### Cẩn thận với `contains` chứa đối số
```scala
// Before
Seq(1, 2, 3).contains("1") // compilable

//  After
Seq(1, 2, 3).contains(1)
```
#### Cách kiểm tra một phần tử  không tồn tại
```scala
// Before
seq.forall(_ != x)

// After
!seq.contains(x)
```
#### Không đếm số lần xuất hiện để kiểm tra sự tồn tại
```scala
// Before
seq.count(p) > 0
seq.count(p) != 0
seq.count(p) == 0

//  After
seq.exists(p)
seq.exists(p)
!seq.exists(p)
```
#### Không sử dụng `filter` để kiểm tra sự tồn tại
```scala
// Before
seq.filter(p).nonEmpty
seq.filter(p).isEmpty

// After
seq.exists(p)
!seq.exists(p)
```
#### Không tìm kiếm để kiểm tra sự tồn tại
```scala
// Before
seq.find(p).isDefined
seq.find(p).isEmpty

// After
seq.exists(p)
!seq.exists(p)
```
### 1.6 Filtering
#### Không sử dụng phép phủ định trong `filter`
```scala
// Before
seq.filter(!p)

// After
seq.filterNot(p)
```
#### Không sử dụng `filter` để đếm phần tử
```scala
// Before
seq.filter(p).length

// After
seq.count(p)
```
#### Không sử dụng `filter` để lấy phần tử đầu tiên
```scala
// Before
seq.filter(p).headOption

// After
seq.find(p)
```
… (còn tiếp) …
## Nguồn tham khảo
https://pavelfatin.com/scala-collections-tips-and-tricks/

#### Biên tập bài viết: Nguyễn Văn Linh