---
title: Làm việc với Slick trong Play Framework
date: 2018-10-30 10:00:00
categories: 
    - play
tags: 
    - basic
---

## I. ORM
ORM (Object Relational Mapping) là một kỹ thuật lập trình ánh xạ từ cơ sở dữ liệu sang đối tượng trong các ngôn ngữ lập trình hướng đối tượng như Java,Scala ... (các table tương ứng với các class, quan hệ giữa các table tương ứng với quan hệ giữa các class).

Sử dụng ORM cho phép người lập trình thao tác với database một cách dễ dàng thông qua các đối tượng mà không cần trực tiếp quan tâm tới database.
Một số framework sử dụng kỹ thuật ORM: Play Scala sử dụng Slick, Sping Java sử dụng Hibernate…

<!-- more -->


### Ưu nhược điểm của ORM:
#### Ưu điểm:
+ Giúp người lập trình tập trung vào hướng đối tượng.
+ Làm việc được với nhiều loại database, nhiều kiểu dữ liệu khác nhau, dễ dàng thay đổi loại database. Các câu lệnh SQL không phụ thuộc vào loại database.
+ Đơn giản, dễ sử dụng: Hỗ trợ HSQL , cung cấp nhiều kiểu API truy vấn.
+ Năng suất hơn: viết code ít hơn, phù hợp với các case
+ Có thể sử dụng lại code.

#### Nhược điểm:
+ Khả năng truy vấn bị hạn chế, nhiều trường hợp ta vẫn phải sử dụng native SQL để truy vấn database.
+ Khó tối ưu câu lệnh SQL (do câu lệnh SQL được ORM tự động sinh ra).

## II. Truy vấn ORM với slick scala:
__Với các bảng có mối quan hệ sau:__

{% asset_img db.png %}

__Mối quan hệ của các bảng:__
+ student - teacher : N-N
+ student - classes : 1-N
+ teacher - classes : 1-N
+ student - student_teacher : 1-N
+ teacher - student_teacher : 1-N

### Ta sẽ thực hiện các trường hợp truy vấn và kết quả từ database khác nhau.
#### Trường hợp 1: Xét bảng student:
```scala
def searchStudentWhere(id: Int): Future[Seq[Tables.Student]] = 
db.run(studentTable.filter(_.studentId === id).sortBy(_.studentId).result)
```
#### Trường hợp 2: Xét bảng classes - student: 1-N
```scala
def searchStudentClassOrder(): Future[Seq[(Student, Option[Classes])]] =
 {
   val action = (for {
     (a, b) <- studentTable joinLeft classesTable on (_.classId === _.classId)
   } yield (a, b)).filter(_._1.studentId ===  id)
db.run(action.sortBy(_._1.studentId).result)
 }
```
#### Trường hợp 3: Xét bảng student - teacher: N-N với bảng trung gian student_teacher
```scala
def searchStudentTeacherWhere(id: Int): Future[Seq[(StudentTeacher, Option[Student], Option[Teacher])]] =
 {
   val action = (for {
     ((a, b), c) <- (studentTeacherTable jo	inLeft studentTable on (_.studentId === _.studentId)) joinLeft teacherTable on (_._1.teacherId === _.teacherId)
   } yield (a, b, c)).filter(_._3.map(_.teacherId) === id).sortBy(_._3.map(_.teacherId))
   db.run(action.result)
 }
```
#### Trường hợp 4: Xét 3 bảng Student - teacher - classes
```scala
def searchStudentClassTeacherWhere(id: Int): Future[Seq[(Tables.StudentTeacher, Option[Student], Option[Teacher], Option[Classes])]] =
 {
   val action = (for {
     (((a, b), c), d) <- ((studentTeacherTable joinLeft
       studentTable on (_.studentId === _.studentId)) joinLeft
       teacherTable on (_._1.teacherId === _.teacherId)) joinLeft
       classesTable on (_._2.map(_.classId.getOrElse(0)) === _.classId)
   } yield (a, b, c, d)).filter(_._4.map(_.classId) === id).sortBy(_._3.map(_.teacherId))
   db.run(action.result)
 }
```

## III. Plain SQL
Đôi khi bạn cần phải viết trực tiếp mã SQL cho một hoạt động không được hỗ trợ trong ORM, thay vì quay lại JDBC bạn có thể sử dụng SQL Plain. Bạn chỉ cần làm rõ các trường của đối tượng với các trường trong bảng database (thay vì ánh xạ chúng) bằng cách sử dụng thư viện GetResult có sẵn trong slick
```scala
import slick.jdbc.GetResult

implicit val GetStudentResults = GetResult(r => Student(r.<<, r.<<, r.<<, r.<<))
implicit val GetClassResults = GetResult(r => Tables.Classes(r.<<, r.<<, r.<<))
implicit val GetTeacherResults = GetResult(r => Teacher(r.<<, r.<<, r.<<, r.<<))
implicit val GetStudentTeacherResults = GetResult(r => StudentTeacher(r.<<, r.<<, r.<<, r.<<))
```
Một số câu truy vấn:
#### Trường hợp 1: Xét bảng student:
```scala
def searchStudentWhereSql(id: Int): Future[Seq[Tables.Student]] = {
 val action = sql"select *from student where student_id = $id order by student_id".as[Student]
 db.run(action)
}
```
#### Trường hợp 2: Xét bảng classes - student: 1-N
```scala
def searchStudentClassWhereSql(id: Int): Future[Vector[(Student, Classes)]] = {
 val action = sql"select *from student a left join classes b on a.class_id = b.class_id where a.student_id = $id order by a.class_id".as[(Student, Classes)]
 db.run(action)
}
```
#### Trường hợp 3: Xét bảng student - teacher: N-N với bảng trung gian student_teacher
```scala
def searchStudentTeacherWhereSql(id: Int): Future[Vector[(Student, StudentTeacher, Teacher)]] = {
 val action =
   sql"""select *
        from student a
        left join student_teacher b on a.student_id = b.student_id
        left join teacher c on b.teacher_id = c.teacher_id
        where c.teacher_id = $id
        order by a.student_id;""".as[(Student, StudentTeacher, Teacher)]
 db.run(action)
}
```
#### Trường hợp 4: Xét 3 bảng Student - teacher - classes
```scala
def searchStudentClassTeacherWhereSql(id: Int): Future[Vector[(Student, StsudentTeacher, Teacher, Classes)]] = {
 val action =
   sql"""select *
        from student a
        left join student_teacher b on a.student_id = b.student_id
        left join teacher c on b.teacher_id = c.teacher_id
        left join classes d on c.class_id = d.class_id
        where a.class_id = $id;""".as[(Student, StudentTeacher, Teacher, Classes)]
 db.run(action)
}
```

## IV. So sánh ORM với Plain SQL
|  -  |     |        Plain SQL        |    ORM     | 
| --- | --- | :------------------- | :-----------: |
|Hiệu năng |     | Nhanh hơn | chậm hơn |
| Tối ưu  |     | Dễ tối ưu hơn        |Khó tối ưu hơn |
|Tính năng động|     | Phải thay đổi code khi thay đổi loại database sử dụng| Tương thích với nhiều loại database mà ORM hỗ trợ, không cần thay đổi code khi thay đổi loại database|
|Khả năng sử dụng lại code|     | Không|Có|

## Nguồn tham khảo:
http://slick.lightbend.com/doc/3.0.0/introduction.html

#### Tác giả: Nguyễn Văn Linh