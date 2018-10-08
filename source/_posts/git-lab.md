---
title: Hướng dẫn sử dụng Gitlab-CI/CD cơ bản
date: 2018-10-06 10:48:07
categories: 
    - ci/cd
tags: 
    - tutorial
---
## 1. Các chức năng chính
* Tự động Build và Deploy lên Staging ___(đã sử dụng được)___
* Tự động Build và Deploy lên Production ___(đang phát triển)___

\- Chức năng tự động Build và Deploy lên Staging sẽ được kích hoạt khi merge code vào
branch "dev" hoặc push code lên branch "dev".

\- Chức năng tự động Build và Deploy lên Production sẽ được kích hoạt khi merge code vào
branch "master" hoặc push code lên branch "master".


## 2. Cách sử dụng
**Bước 1: merge code vào branch "dev" hoặc push code lên branch "dev"**

**Bước 2: chờ quá trình Build - Deploy diễn ra**
___Bước 2.1:___ Đầu tiên, chúng ta vào mục "Pipelines" để kiểm tra trạng thái của quá trình. 

{% asset_img image.69CEQZ.png %}

___Bước 2.2:___ Nhấn vào "running" để đi tới màn hình chi tiết của quá trình.

{% asset_img image.FNDBQZ.png %}

___Bước 2.3:___ ​ Nhấn vào "staging" để đi tới màn hình console - nơi hiển thị chi tiết quá trình build và deploy dự án lên Staging.

{% asset_img image.MPJ6PZ.png %}



\- Sau bước này, chúng ta sẽ có kết quả của quá trình Build -> Deploy. Sẽ có hai trường hợp xảy ra:
* Quá trình xảy ra lỗi: Gitlab-CI/CD sẽ báo trạng thái của quá trình sẽ hiển thị giống như {% asset_img center image.GDDMQZ.png "image" %}. Hoặc hiển thị "ERROR: Job failed: exit code 1" trên màn hình console ở dòng cuối cùng. 
* Quá trình diễn ra thành công: trạng thái sẽ là {% asset_img center image.U7CBQZ.png %}.

**Bước 3: xử lý kết quả của quá trình Build - Deploy**

___Bước 3.1:___ N​ếu kết quả trả về là thành công . Chúng ta sẽ lên Staging server để xem
kết quả, công việc triển khai coi như xong. Ngoài ra, chúng ta có thể xem chi tiết hơn bằng
cách:

\- Vào mục "Builds" để xem tất cả các quá trình Build - Deploy đã thực hiện (bao gồm cả quá
trình thực thi lại một lần Build - Deploy nào đó)

{% asset_img image.VKRQQZ.png %}

\- Vào mục "Environments" để xem các môi trường đã triển khai. Ở đây, chúng ta mới triển
khai lên môi trường Staging. Môi trường Production sẽ được triển khai sau...

{% asset_img image.P6KJQZ.png %}

___Bước 3.2:___ Nếu kết quả trả lỗi, chúng ta nên đọc lại phần thông tin hiển thi ở màn hình console trước để tìm ra nguyên nhân. Sau đó đi tới mục "related-code" để xem chi tiết đoạn code nào đã gây lỗi cho hệ thống. 

{% asset_img image.BQBNQZ.png %}


