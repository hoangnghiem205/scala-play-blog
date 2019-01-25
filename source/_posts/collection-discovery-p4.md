---
title: Scala Collections Tips & Tricks (Phần IV)
date: 2018-12-06 10:00:00
categories: 
    - scala 
tags: 
    - discovery
---

- Phần I: Sequences (Creation & Length & Equality)
- Phần II: Sequences (Indexing & Existence & Filtering)
- Phần III: Sequences (Sorting & Reduction & Matching & Rewriting)
- __Phần IV: Sets__
- Phần V: Options
- Phần VI: Maps

## 2.Sets
#### Không sử dụng `sameElements` để so sánh các collection không có thứ tự
```scala
// Before
set1.sameElements(set2)

// After
set1 == set2
```
<!-- more -->

#### Sử dụng `set` như một giá trị hàm
```scala
// Before (set: Set[Int])
Seq(1, 2, 3).filter(set(_))
Seq(1, 2, 3).filter(set.contains)

// After
Seq(1, 2, 3).filter(set)
```
#### Không tính toán theo cách thủ công
```scala
// Before
set1.filter(set2)

// After
set1.intersect(set2) // or set1 & set2
```
#### Dùng `diff` sẽ đơn giản hơn
```scala
// Before
set1.filterNot(set2)

// After
set1.diff(set2) // or set1 &~ set2
… (còn tiếp) …
## Nguồn tham khảo
https://pavelfatin.com/scala-collections-tips-and-tricks/

#### Biên tập: Nguyễn Văn Linh