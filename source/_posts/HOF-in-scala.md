---
title: Higher Order Functions trong Scala
date: 2018-10-02 20:08:24
categories: 
    - scala
tags: 
    - basic
---
## Định nghĩa Higher Order Function
`Higher Order Function` là function thỏa mãn ít nhất một trong hai điều kiện :
- Có ít nhất một tham số truyền vào là một function khác
- Kết quả trả về của `HOF` đó là một function khác.

Với Higher Order Function, tính trừu tượng hóa, tái sử dụng chính là điểm mạnh.

## Tính trừu tượng hóa (Abstraction) 
Hãy nghĩ đến dây chuyền lắp ráp một chiếc xe. Dây chuyền gồm bốn bộ phận:
- Bộ phận tạo bánh xe
- Bộ phận tạo khung xe
- Bộ phận tạo động cơ
- Bộ phận lắp ráp

Bộ phận tạo bánh xe sẽ tạo ra bánh xe và chuyển qua bộ phận lắp. Tương tự như vậy, bộ phận tạo khung xe và bộ phân tạo động cợ sẽ tạo ra khung và động cơ, sau đó chuyển sang cho bộ phân lắp ráp. Bộ phận lắp ráp không cần phải biết bánh xe, khung xe hay động cơ xe được tạo ra như thế nào. Chức năng của bộ phận lắp ráp chỉ là ghép các thành phần để tạo ra chiếc xe mà thôi. Bộ phận lắp ráp chính là một HOF, nó nhận output của ba bộ phận còn lại để tạo ra output cho riêng nó. Có thể diễn giải như thế này:

```scala
class BanhXe{} 
class KhungXe{} 
class DongCoXe{} 

// ham tao banh xe 
def taoBanhXe() = { 
  val banhXe = new BanhXe 
  banhXe 
} 

// ham tao khung xe 
def taoKhungXe() = { 
  val khungXe = new KhungXe 
  khungXe 
} 

// ham tao dong co xe 
def taoDongCoXe() = { 
  val dongCoXe = new DongCoXe 
  dongCoXe 
} 

// HOF function 
def lapRapXe(taoBanhXe(), taoKhungXe(), taoDongCoXe()) 
```

Từ ví dụ trên, chúng ta có thể hiểu tính trừu tượng hóa của HOF giúp che giấu chi tiết bên trong một function, làm giảm sự phức tạp. Vấn đề sẽ được xử lý ở một tầng cao hơn, trừu tượng hơn.
 
## Tính tái sử dụng (Reusable)
Cũng sử dụng ví dụ về dây chuyền tạo ra một chiếc xe. Chúng ta xây dựng một dây chuyền sản xuất xe máy. Tuy nhiên, nếu phải tạo ra thêm một dây chuyền sản xuất xe hơi thêm vào thì sẽ như thế nào ? Tạo ra một dây chuyền mới là giải pháp đơn giản nhất nhưng lại tốn kém về chi phí. Chúng ta có thể bổ sung thêm cho bộ phận tạo bánh xe máy khả năng tạo ra bánh xe hơi. Tương tự như vậy, bộ phận tao khung xe và động cơ đều trang bị thiết bị tạo ra đồng thời linh kiện cho xe máy lẫn xe hơi. Như vậy, ba bộ phân trên đều được sử dụng tùy theo yêu cầu tạo ra sản phẩm. Đó chính là tính tái sử dụng.

## Cách sử dụng HOF trong Scala
Một bài toán đơn giản được đặt ra, hãy hình dung bạn đang là một ông chủ và muốn tăng lương cho nhân viên của mình. Tăng lương là việc bạn nhân số lương hiện tại của nhân viên với một hệ số nhất định. Bạn nghĩ rằng, việc tăng lương cần linh động do đó phải tạo ra nhiều cách tăng lương khác nhau. Với bài toán trên, chúng ta sẽ đi giải quyết như sau:

- Để tiện cho việc quản lý vấn đề tăng lương, bạn sử dụng một **Object** có tên là **SalaryRaiser**. 
- Bạn có 3 mức tăng lương khác nhau, theo thứ tự tăng dần là: **smallPromotion()**, **greatPromotion()** và **hugePromotion()**.

- Ở mức smallPromotion, hệ số là 1.1
- Ở mức smallPromotion, hệ số là logarit cơ số 10 của số lương hiện tại ứng với nhân viên đó.
- Ở mức smallPromotion, hệ số là số lương hiện tại của nhân viên đó.
 
Theo cách thông thường, chúng ta sẽ dùng một vòng for để cập nhật giá trị lương của từng nhân viên. Sau đó, trả về danh sách lương của nhân viên sau khi đã cập nhật. Các bạn xem code phía dưới.

```scala
object SalaryRaiser {

  def smallPromotion(salaries: Array[Double]): Array[Double] = {
    for (i <- salaries.indices) {
      salaries(i) = salaries(i) * 1.1
    }
    salaries
  }

  def greatPromotion(salaries: Array[Double]): Array[Double] = {
    for (i <- salaries.indices) {
      salaries(i) = salaries(i) * math.log(salaries(i))
    }
    salaries
  }

  def hugePromotion(salaries: Array[Double]): Array[Double] = {
    for (i <- salaries.indices) {
      salaries(i) = salaries(i) * salaries(i)
    }
    salaries
  }

}
```
 
Với `HOF`, việc viết code trở nên dễ dàng và ngắn gọn hơn khá nhiều. Ở đây, chúng ta sẽ sử dụng một HOF đó là `map()`. Chúng ta truyền vào map() một function, chính function này sẽ giúp chúng ta thay đổi lương của mỗi nhân viên theo hệ số  mong muốn.  Hàm map() đã thể hiện tính linh hoạt thông qua việc định nghĩa cách xử lý từng phần tử trong Array.

```scala
object SalaryRaiser {

  def smallPromotion(salaries: Array[Double]): Array[Double] = {
    salaries.map(salary => salary * 1.1)
  }

  def greatPromotion(salaries: Array[Double]): Array[Double] = {
    salaries.map(salary => salary * math.log(salary))
  }

  def hugePromotion(salaries: Array[Double]): Array[Double] = {
    salaries.map(salary => salary * salary)
  }

}
```

Hiện nay đối với **Scala**, `map()` là một trong những `HOF` được dùng nhiều nhất. Ngoài ra, các bạn có thể tìm hiểu thêm về các HOF khác như `filter()`, `flatMap()` …

## Nguồn tham khảo
1. https://discuss.grokking.org/t/higher-order-functions-la-gi-va-d-c-s-d-ng-nh-th-nao/309

2. https://docs.scala-lang.org/tour/higher-order-functions.html