---
title: Scala Collections Tips & Tricks (Phần I)
date: 2018-11-22 10:00:00
categories:
    - scala 
tags:
    - discovery
---
Bài blog này sẽ giới thiệu tới các bạn một danh sách những cách `hay ho` để tối ưu việc sử dụng `Scala Collections`. Vì bài viết khá dài nên mình sẽ chia thành 6 phần để các bạn tiện theo dõi. Và dưới đây là mục lục các bài viết:

Bài blog này sẽ giới thiệu tới các bạn một danh sách những cách `hay ho` để tối ưu việc sử dụng `Scala Collections`. Vì bài viết khá dài nên mình sẽ chia thành 6 phần để các bạn tiện theo dõi. Và dưới đây là mục lục các bài viết:

- **Phần I: Sequences (Creation & Length & Equality)**
- Phần II: Sequences (Indexing & Existence & Filtering)
- Phần III: Sequences (Sorting & Reduction & Matching & Rewriting)
- Phần IV: Sets
- Phần V: Options
- Phần VI: Maps

## 1. Sequences
### 1.1 Creation
Tạo `collection` rỗng một cách rõ ràng.
```scala
// Before
Seq[T]()

// After
Seq.empty[T]
```
<!-- more -->

Một số `collection` không thể thay đổi cung cấp các tập rỗng. Tuy nhiên không phải tất cả các phương thức kiểm tra độ dài `collection` đã tạo. Do đố làm rỗng `collection` chúng ta có thể tiết kiệm bộ nhớ bằng cách sử dụng lại `collecion` rỗng.

Có thể áp dụng với: `Set,Option,Map,Iterator`.
### 1.2 Length
Ưu tiên `length` hơn `size` cho mảng.
```scala
// Before
array.size

// After
array.length
```
Trong khi `size` và `length` là 2 từ đồng nghĩa, Scala 2.11 `Array.size` được gọi thông qua chuyển đổi ngầm, để các đối tượng trung gian được tạo cho mọi phương thức. Trừ khi bạn cho phép `escape analysis` trong JVM, những đối tượng này có khả năng làm giảm hiệu suất (đặc biệt trong vòng lặp).

#### Không dùng phép phủ định
```scala
// Before
!seq.isEmpty
!seq.nonEmpty

// After
seq.nonEmpty
seq.isEmpty
```
Không sử dụng phủ định sẽ trực quan hơn.

Có thể áp dụng với: `Set,Option,Map,Iterator`.

#### Không sử dụng `length` để kiểm tra rỗng
```scala
// Before
seq.length > 0
seq.length != 0
seq.length == 0

// After
seq.nonEmpty
seq.nonEmpty
seq.isEmpty
```
#### Không sử dụng `length` để so sánh độ dài

```scala
// Before
seq.length > n
seq.length < n
seq.length == n
seq.length != n

// After
seq.lengthCompare(n) > 0
seq.lengthCompare(n) < 0
seq.lengthCompare(n) == 0
seq.lengthCompare(n) != 0
```
#### Không sử dụng `exist` để kiểm tra rỗng
```scala
// Before
seq.exists(_ => true)
seq.exists(const(true))

// After
seq.nonEmpty
```
### 1.3 Equality
#### Không sử dụng `==` để so sánh nội dung các mảng
 ```scala
 // Before
array1 == array2

// After
array1.sameElements(array2)
```
#### Không so sánh giữa các collection khác loại.
```scala
// Before
seq == set

// After
seq.toSet == set
```
#### Không sử dụng `sameElements` để so sánh các collection bình thường.
```scala
// Before
seq1.sameElements(seq2)

// After
seq1 == seq2
```
#### Không so sánh tương đối một cách thủ công
```scala
// Before
seq1.corresponds(seq2)(_ == _)

// After
seq1 == seq2
```
… (còn tiếp) …
## Nguồn tham khảo
https://pavelfatin.com/scala-collections-tips-and-tricks/

#### Biên tập bài viết: Nguyễn Văn Linh