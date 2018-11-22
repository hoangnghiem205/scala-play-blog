---
title: trying_cala_basic
date: 2018-11-23 01:25:20
categories: 
    - play
tags: 
    - basic
---

## CÁC KHÁI NIỆM CƠ BẢN

### Dùng thử scala trên trình duyệt

Bạn có thể chạy Scala trên trình duyệt với ScalaFiddle.

1. Đi tới https://scalafiddle.io.
2. Paste cái này println("Hello, world!") vào phần bên trái màn hình.
3. Ấn vào nút  “Run”. Đầu ra xuất hiện ở phần màn hình bên phải.
Đây là một cách dễ dàng, cách thiết lập về zero để thử với các đoạn code Scala.
Nhiều ví dụ về code trong tài liệu này cũng được tích hợp với ScalaFiddle, để bạn có thể thử trực tiếp với nó chỉ đơn giản bằng cách click vào nut “Run”.

### Biểu thức (Expressions)

Biếu thức là câu lệnh tính toán được.
```scala
1 + 1
```

Bạn có thể xuất kết quả của biểu thức bằng cách sử dụng **println**.
```scala
println(1) // 1
println(1 + 1) // 2
println("Hello!") // Hello!
println("Hello," + " world!") // Hello, world!
```

### Các giá trị (Values)

Bạn có thể đặt tên cho kết quả của biểu thức bằng từ khóa **val**.
```scala
val x = 1 + 1
println(x) // 2
```

Kết quả được đặt tên, giống như **x** ở đây, được gọi là giá trị. Tham chiếu một giá trị không tính nó.
Không thể gán lại giá trị.
```scala
x = 3 // This does not compile.
```

kiểu của giá trị có thể được suy luận ra, nhưng bạn cũng có thể khai báo rõ kiểu của nó như thế này:
```scala
val x: Int = 1 + 1
```

### Các khối code (Blocks)

Bạn có thể kết hợp các biểu thức và đoạn code bằng cách bao ngoài chúng bằng {}. Chúng ta gọi nó là block.
Kết quả của biểu thức cuối cùng trong khối chính là kết quả của khối bao ngoài đó , giống như dưới đây.
```scala
println({
  val x = 1 + 1
  x + 1
}) // 3
```

### Các hàm

Hàm là biểu thức có tham số.
Bạn có thể định nghĩa một hàm ẩn danh (i.e. no name) trả về một số nguyên nhất định cộng thêm một:
```scala
(x: Int) => x + 1
```

Ở bên trái của dấu **=>** là một danh sách các tham số. bên phải là biểu thức liên quan đến các tham số đó.
Bạn cũng có thể đặt tên cho các hàm.

```scala
val addOne = (x: Int) => x + 1
println(addOne(1)) // 2
```

Hàm có thể lấy nhiều tham số.
```scala
val add = (x: Int, y: Int) => x + y
println(add(1, 2)) // 3
```

Hoặc nó có thể không có tham số.
```scala
val getTheAnswer = () => 42
println(getTheAnswer()) // 42
```

### Các phương thức (Methods):

Khi nhìn vào phương thức sẽ rất giống các hàm, đặc biệt là các hành động của nó, Nhưng thật ra có một số khác biệt giữa chúng .
Các phương thức được định nghĩa với từ khóa **def**. Sau **def** là tên của phương thức , rồi đến danh sách tham số, một kiểu trả về, và phần thân của nó.
```scala
def add(x: Int, y: Int): Int = x + y
println(add(1, 2)) // 3
```

Lưu ý kiểu trả về được khai báo sau danh sách tham số và dấu hai chấm **: Int**.
Methods can take multiple parameter lists.
```scala
def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier
println(addThenMultiply(1, 2)(3)) // 9
```

Hoặc không có tham số nào cả.
```scala
def name: String = System.getProperty("user.name")
println("Hello, " + name + "!")
```

Có một số khác biệt nữa, nhưng bây giờ, bạn có thể nghĩ chúng là cái gì đó giống như các hàm.
Các phương thức cũng có thể có nhiều dòng biểu thức.
```scala
def getSquareString(input: Double): String = {
  val square = input * input
  square.toString
}
```

Biểu thức cuối cùng trong phần thân là giá trị trả về của phương thức. (Scala không có từ khóa **return**, nhưng nó hiếm khi được sử dụng.)

### Các lớp

Bạn có thể định nghĩa các lớp với từ khóa **class**, đứng sau nó là tên và các tham số của hàm khởi tạo.
```scala
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit =
    println(prefix + name + suffix)
}
```

Kiểu trả về của phương thức **greet** là **Unit**, nghĩa là không có gì để trả về. Nó được sử dụng tương tự như **void** trong Java và C. (Điểm khác biệt là mọi biểu thức của Scala phải có một số giá trị, đó là một giá trị singleton của kiểu **Unit**, written (). Nó không chứa thông tin.)
Bạn có thể tạo thể hiện của một lớp với từ khóa ** new**.
```scala
val greeter = new Greeter("Hello, ", "!")
greeter.greet("Scala developer") // Hello, Scala developer!
```

Chúng ta sẽ tìm hiểu sâu hơn về  các Class sau.

### Các Case Class

Scala có một kiểu đặc biệt của class gọi là một “case” class. Theo mặc định, case classes không thay đổi và được so sánh theo giá trị. Bạn có thể định nghĩa case classes với từ khóa **case class**.
```scala
case class Point(x: Int, y: Int)
```

Bạn có thể khởi tạo các class không có từ khóa **new**.
```scala
val point = Point(1, 2)
val anotherPoint = Point(1, 2)
val yetAnotherPoint = Point(2, 2)
```

Và chúng được so sánh theo giá trị.
```scala
if (point == anotherPoint) {
  println(point + " and " + anotherPoint + " are the same.")
} else {
  println(point + " and " + anotherPoint + " are different.")
} // Point(1,2) and Point(1,2) are the same.

if (point == yetAnotherPoint) {
  println(point + " and " + yetAnotherPoint + " are the same.")
} else {
  println(point + " and " + yetAnotherPoint + " are different.")
} // Point(1,2) and Point(2,2) are different.
```

Có rất nhiều trường hợp khác mà chúng tôi muốn giới thiệu và chúng tôi tin rằng bạn sẽ thích nó! Chúng ta sẽ tìm hiểu sâu hơn về chúng ở các phần sau.

### Các đối tượng (Objects)

Các đối tượng là các thực thể đơn lẻ được định nghĩa riêng bởi chính nó. Bạn có thể nghĩ nó là singletons trong các lớp của riêng nó.
Bạn có thể định nghĩa các đối tượng bằng từ khóa **object**.
```scala
object IdFactory {
  private var counter = 0
  def create(): Int = {
    counter += 1
    counter
  }
}
```

Bạn có thể truy cập bằng cách tham chiếu tên của nó.
```scala
val newId: Int = IdFactory.create()
println(newId) // 1
val newerId: Int = IdFactory.create()
println(newerId) // 2
```

Chúng tôi sẽ giúp bạn hiểu rõ hơn về đối tượng trong các bài sau.

### Các Trait

Trait là các kiểu chứa các trường và phương thức nhất định. Có thế kết hợp nhiều trait.
Bạn có thể định nghĩa các trait với từ khóa **trait**.
```scala
trait Greeter {
  def greet(name: String): Unit
}
```

Trait cũng có thể triển khai mặc định.
```scala
trait Greeter {
  def greet(name: String): Unit =
    println("Hello, " + name + "!")
}
```

Bạn có thể mở rộng các trait với từ khóa **extends** và ghi đè với từ khóa **override**.
```scala
class DefaultGreeter extends Greeter

class CustomizableGreeter(prefix: String, postfix: String) extends Greeter {
  override def greet(name: String): Unit = {
    println(prefix + name + postfix)
  }
}

val greeter = new DefaultGreeter()
greeter.greet("Scala developer") // Hello, Scala developer!

val customGreeter = new CustomizableGreeter("How are you, ", "?")
customGreeter.greet("Scala developer") // How are you, Scala developer?
```

Ở đây, **DefaultGreeter** chỉ mở rộng một trait duy nhất, nhưng nó có thể mở rộng nhiều trait.
Chúng tôi sẽ đề cập đến các trait ở các bài sau.

### Phương thức Main

Phương thức main là điểm vào của một chương trình. Máy ảo Java yêu cầu phương thức main được đặt tên là **main** và lấy một đối số, một chuỗi của các chuỗi.
Sử dụng một đối tượng, bạn có thể định nghĩa một phương thức main như sau:
```scala
object Main {
  def main(args: Array[String]): Unit =
    println("Hello, Scala developer!")
}
```










