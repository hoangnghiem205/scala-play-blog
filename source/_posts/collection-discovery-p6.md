---
title: Scala Collections Tips & Tricks (Phần VI)
date: 2018-12-13 10:00:00
categories: 
    - scala 
tags: 
    - discovery
---

- Phần I: Sequences (Creation & Length & Equality)
- Phần II: Sequences (Indexing & Existence & Filtering)
- Phần III: Sequences (Sorting & Reduction & Matching & Rewriting)
- Phần IV: Sets
- Phần V: Options
- __Phần VI: Maps__

## 4 Map
### Không tìm kiếm giá trị theo cách thủ công
```scala
// Before
map.find(_._1 == k).map(_._2)

// After
map.get(k)
```
 <!-- more -->

### Không sử dụng `get` khi cần giá trị thô
```scala
// Before
map.get(k).get

// After
map(k) 
```
### Sử dụng `get` thay vì `lift`
```scala
// Before
map.lift(k)

// After
map.get(k)
```
### Không gọi `get` cùng với `getOrElse`
```scala
// Before
map.get(k).getOrElse(z)

// After
map.getOrElse(k, z)
```
### Sử dụng `Map` thể hiện dưới dạng một giá trị hàm
```scala
// Before (map: Map[Int, T])
Seq(1, 2, 3).map(map(_))

// After
Seq(1, 2, 3).map(map)
```
### Không trích xuất các khóa theo cách thủ công
```scala
// Before
map.map(_._1)
map.map(_._1).toSet
map.map(_._1).toIterator

// After
map.keys
map.keySet
map.keysIterator
```
### Không trích xuất giá trị theo cách thủ công
```scala
// Before
map.map(_._2)
map.map(_._2).toIterator

// After
map.values
map.valuesIterator
```
### Cẩn thận khi sử dụng `filterKeys`
```scala
// Before
map.filterKeys(p)

// After
map.filter(p(_._1))
```
### Cẩn thận khi sử dụng `mapValues`
```scala
// Before
map.mapValues(f)

// After
map.map(f(_._2))
```
### Không lọc ra các `key` theo cách thủ công
```scala
// Before
map.filterKeys(!seq.contains(_))

// After
map -- seq
```
### Sử dụng toán tử gán để gán lại `map`
```scala
// Before
map = map + x -> y
map1 = map1 ++ map2
map = map - x
map = map -- seq

// After
map += x -> y
map1 ++= map2
map -= x
map --= seq
```
## Nguồn tham khảo
https://pavelfatin.com/scala-collections-tips-and-tricks/

#### Biên tập bài viết: Nguyễn Văn Linh