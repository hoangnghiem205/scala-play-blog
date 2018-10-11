---
title: Tích hợp Gitlab-CICD vào dự án thực tế
date: 2018-10-11 14:27:20
categories: 
    - ci/cd
tags: 
    - tutorial
---

&nbsp;&nbsp;&nbsp;&nbsp;Gần đây, team Best Solution đã tích hợp được Gitlab-CICD vào dự án ___Green-Blue___ để tự động hóa quá trình Build, Deploy ứng dụng lên Staging. Dưới đây là hướng dẫn về cách tích hợp Gitlab-CICD cho những dự án khác. Trong hướng dẫn này, mình sử dụng GitLab Community Edition 8.16.3 và Gitlab Runner 1.11.1

### __1. Tích hợp Gitlab Runner và thêm biến môi trường cho dự án trên Gitlab__
&nbsp;&nbsp;&nbsp;&nbsp;Để có thể tiến hành quá trình Build-Deploy, Gitlab sử dụng một server có tên là Runner. Tại đây, ứng dụng của chúng ta sẽ được Build,Test, sau đó được Deploy lên server. 

&nbsp;&nbsp;&nbsp;&nbsp;Hiện tại, Gitlab cung cấp các Shared Runner miễn phí cho chúng ta. Nhưng vì yếu tố bảo mật và đảm bảo hiệu năng, chúng ta nên tự cài đặt một Runner Server riêng phục vụ cho các dự án của công ty.

&nbsp;&nbsp;&nbsp;&nbsp;Trong team Best Solution, mình đã cài đặt một Runner Server có tên là ___the runner___. Chúng ta chỉ cần tích hợp Runner vào dự án là có thể sử dụng được.

### __1.1 Tích hợp Gitlab Runner__
Chúng ta vào mục Runner (1) của dự án, sẽ thấy màn hình như hình phía dưới.

{% asset_img cicd1.png %}

&nbsp;&nbsp;&nbsp;&nbsp;Ở đây có hai phần, ___Specific Runners___ và ___Shared Runners___. Như đã nói ở trên, chúng ta chỉ quan tâm tới Specific Runners. Nhìn vào (2), các bạn sẽ thấy một Runner mà mình đã cài đặt sẵn. Để tích hợp Runner vào dự án, chỉ cần nhấn vào nút ___Enable for this project___ như (3) là xong.

### __1.2 Thêm biến môi trường cho dự án__
Để thêm biến môi trường, chúng ta vào mục Variables (1).
{% asset_img cicd2.png %}

&nbsp;&nbsp;&nbsp;&nbsp;Đây chính là nơi các bạn khai báo biến môi trường với Gitlab và Runner sẽ sử dụng những biến môi trường này. Có một câu hỏi là: tại sao lại cần biến môi trường ?
&nbsp;&nbsp;&nbsp;&nbsp;Mình sẽ lấy ví dụ để trả lời câu hỏi này, các bạn nhìn vào (3). Ở đây mình khai báo một biến môi trường với tên là SSH_PRIVATE_KEY và giá trị của nó sẽ là tất cả các ký tự trong file key mà các bạn sử dụng để ssh lên server khi tiến hành Deploy ứng dụng. Nếu chúng ta không khai báo một key ở đây, thì Runner Server sẽ không thể tiến hành quá trình Deploy được.
&nbsp;&nbsp;&nbsp;&nbsp;Với một dự án mới, sẽ không có biến môi trường nào hiển thị ở đây cả. Các bạn sẽ phải tự thêm vào. Đối với team Best Solution, chỉ cần thêm biến SSH_PRIVATE_KEY là đủ. 

### __2. Viết script triển khai ứng dụng lên server__
&nbsp;&nbsp;&nbsp;&nbsp;Trên server, chúng ta tạo một thư mục để triển khai ứng dụng. Ở đây, mình lấy ví dụ với ứng dụng Gree-Blue, thư mục sẽ giống như hình bên dưới:
__green-blue-cicd/__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ __config/__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ database.conf
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ __log/__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ application.log
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ __script/__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ deploy.sh
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ __source/__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ green-blue-STAGING-20181005172440/
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ ...
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ __version/__
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ green-blue-STAGING-20181005172440.zip
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|\__ ...

Mình sẽ giới thiệu qua mục đích của từng folder trong thư mục chính __green-blue-cicd/.__

- Đầu tiên là folder _**config/**_, đây là nơi chứa config của ứng dụng. Hiện tại, mình chỉ để config của database trong đó. Chúng ta cần một file config database mẫu để dùng cho quá trình triển khai ứng dụng.
- Thứ hai là folder _**log/**_, đây là nơi chứa log của ứng dụng trong quá trình chạy.
- Thứ ba là folder _**script/**_, mình để script triển khai ứng dụng ở đây.
- Folder _**source/**_ là nơi chứa mã nguồn được giải nén ra từ các phiên bản của ứng dụng nằm bên trong folder _**version/.**_

Điều quan tâm nhất là script ___deploy.sh___ sẽ được viết như thế nào ...
```bash
#!/bin/bash
# define app name
APP_NAME="green-blue"

# unzip source-code
cd /root/web/green-blue-cicd/version
unzip $1.zip -d ../source/

# re-config application
cd /root/web/green-blue-cicd/source/$1/conf
mv mailer.conf.example mailer.conf
mv silhouette.conf.example silhouette.conf
cp /root/web/green-blue-cicd/config/database.conf ./

# kill process of prev-version
pid=$(ps -p $(lsof -ti tcp:9000) o pid=)
kill -9 $pid

# run server & write log file
cd /root/web/green-blue-cicd/source/$1/bin/
nohup ./$APP_NAME -Dplay.http.secret.key=123123123 -Dplay.evolutions.db.default.autoApply=true -Dhttp.port=9000 > /root/web/green-blue-cicd/log/application.log &
```
&nbsp;&nbsp;&nbsp;&nbsp;Hình ảnh phía trên là nội dung file deploy.sh dùng để triển khai Green-Blue. Trong đó, ***$1*** là tham số truyền vào khi chạy script deploy.sh, ở đây ***$1*** đại diện cho tên của version muốn triển khai. Chúng ta có thể thấy, nội dung file gồm 4 phần:
\- (1) Giải nén source-code
\- (2) Cấu hình cho ứng dụng, trong đó có bước lấy file database.conf từ file mẫu ban đầu.
\- (3) Bỏ bản triển khai trước đó, nó đang chạy ở cổng 9000
\- (4) Triển khai một version mới và ghi log vào application.log

&nbsp;&nbsp;&nbsp;&nbsp;Để triển khai CICD cho một dự án khác, các bạn chỉ cần giữ nguyên cấu trúc thư mục và thay đổi nội dung các file bên trong của nó. Chính xác là chỉ cần thay đổi nội dung file database.conf và file deploy.sh. Nội dung file database.conf sẽ được thay đổi tùy vào các dự án khác nhau. Đối với deploy.sh, chúng ta cần làm 2 việc:
\- Thay tên của các đường dẫn
\- Thay tên của ứng dụng trong bước (4).

### __3. Viết file .gitlab.yml__
&nbsp;&nbsp;&nbsp;&nbsp;Đầu tiên, chúng ta sẽ đi xem .gitlab-ci.yml của dự án Green-Blue [ở đây](http://git.ows.vn/linhnh/green-blue-system/blob/bf3c2a6e63d57ddd9d618421666cc95f3019b49b/.gitlab-ci.yml).
Chúng ta sẽ thấy có các phần chính sau:
- image: docker image được dùng cho quá trình Build - Deploy
- stages: nơi liệt kê các quá trình được thực thi
- variables: nơi khai báo các biến  môi trường 
- cache: nơi khai báo các thư mục mà bạn muốn cache lại sau mỗi lần Build - Deploy
- staging: quá trình triển khai ứng dụng lên STAGING

&nbsp;&nbsp;&nbsp;&nbsp;Khi viết .gitlab-ci.yml cho một dự án mới, chúng ta chỉ cần quan tâm tới staging, đây là nơi thực thi các câu lệnh tiến hành quá trình Build - Deploy ứng dụng. Các câu lệnh thực thi được chia làm hai phần:

***a) before_script***: các câu lệnh chuẩn bị cho quá trình Deploy, bao gồm các bước:
(1) Tạo tên version mới cho ứng dụng
```yml
# Create VERSION_NAME
## Set TIME ZONE
- TZ=Asia/Ho_Chi_Minh
- ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
## Set VERSION_NAME
- export VERSION_NAME=$(date +'%Y%m%d%H%M%S')
- echo "VERSION_NAME is green-blue-STAGING-${VERSION_NAME}"
- apt-get update -y
```
(2) Cài đặt sbt
```yml
# Install SBT
- echo "deb http://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list
- apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823
- apt-get update -y
- apt-get install sbt -y
```
(3) Thay đổi tên version cho ứng dụng
```yml
# Change BUILD VERSION
- sed -i 's/BUILD_VERSION/'"STAGING-${VERSION_NAME}"'/g' build.sbt
- sed -i 's/BUILD_VERSION/'"green-blue-STAGING-${VERSION_NAME}"'/g' public/javascripts/zxcvbnShim.js
```
(4) Thực hiện quá trình Build
```yml
- sbt dist
```
(5) Cài đặt ssh
```yml
# Setup SSH
- 'which ssh-agent || ( apt-get update -y && apt-get install openssh-client -y )'
# Run ssh-agent (inside the build environment)
- eval $(ssh-agent -s)
# Add the SSH key stored in SSH_PRIVATE_KEY variable to the agent store
- ssh-add <(echo "$SSH_PRIVATE_KEY")
- mkdir -p ~/.ssh
- '[[ -f /.dockerenv ]] && echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config'
```
(6) Chuyển mã nguồn sau khi Build lên Staging server
```yml
# Send FILE to remote server
- scp target/universal/green-blue-STAGING-${VERSION_NAME}.zip root@133.18.199.250:/root/web/green-blue-cicd/version
```

___b) script___: các câu lệnh của quá trình Deploy, đây là việc ssh lên Staging và thực thi deploy.sh
```yml
- echo "DEPLOY to STAGING server ..."
# Deploy
- ssh root@133.18.199.250 "sh /root/web/green-blue-cicd/script/deploy.sh green-blue-STAGING-${VERSION_NAME}"
```

Chúng ta sẽ phải thay đổi một chút trong khi viết file .gitlab-ci.yml mới.
Đối với ***before_script***, chúng ta chỉ cần thay đổi ở các bước (3) và (6):

Với bước (3)
* Thay đổi version := "1.x.x" trong ___build.sbt___ thành version := "BUILD_VERSION"
* Thêm câu lệnh console.log("Current version is " + "BUILD_VERSION") vào đầu file ___zxcvbnShim.js___

Với bước (6)
* thay đổi cái tên green-blue trong đường dẫn target/universal/green-blue-STAGING-${VERSION_NAME}.zip thành tên dự án của bạn.
* thay đổi đường dẫn /root/web/green-blue-cicd/version cho đúng với đường dẫn với thư mục version/ trên server của bạn.

Đối với ___script___:
* Thay đổi đường dẫn /root/web/green-blue-cicd/script/deploy.sh cho đúng với đường dẫn tới file **deploy.sh** của bạn trên server.
* Thay đổi tên green-blue trong green-blue-STAGING-${VERSION_NAME} thành tên dự án của bạn.

&nbsp;&nbsp;&nbsp;&nbsp;Đến đây, chúng ta chỉ việc push những gì vừa làm lên branch ___dev___ là hoàn thành xong việc tích hợp Gitlab-CICD cho dự án mới.






