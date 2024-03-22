# LFI-And-Typical-Causes
Lỗi Local File Inclusion (LFI) xảy ra khi một ứng dụng web cho phép người dùng truy nhập các file hệ thống hoặc tệp tin nhạy cảm khác trên server, thông qua việc không chặn đúng cách nhập liệu từ người dùng. Đây là một số ví dụ dạng dòng code hoặc mô hình phát triển có thể gây ra lỗi LFI:

1. Dùng luôn input đầu vào và k kiểm soát để đọc filr
```
<?php 
  $file =  $_GET['file];
  include($file)
?>
```
Trong đoạn PHP trên , biến ``` $_GET['file]; ```lấy giá trị từ tham số file trong URL và trực tiếp dùng luôn cho include() mà không filter hay kiểm duyệt gì cả 

Khi URL http://example/com/script.php?file=content.php
thì ``` $_GET['file]; ``` sẽ nhận giá trị là content.php
-> sau đó biến $file được gán giá trị là content.php
lệnh include($file) sẽ tìm file có tên content.php và output ra nội dung file đó
-> rủi ro về việc user có thể trỏ đến bất kì file nào mà họ muốn

2. Linked File Path
```
<?php
  $filepath = 'includes/' . $_GET['page'];  
  include($filepath);
?>
```
này là tượng đài của url lfi với các ví dụ 

http://example/com/script.php?file=content.php

Nối chuỗi input của người dùng vào chạy luôn

3. Ghi đè biến 
```
<?php
  $template = 'default.php';
  if(isset($_GET['template'])){
    $template = $_GET['template'];
  }
  include('/templates/' . $template);
?>
```
Có nỗ lực đặt file mặc định nhưng không đáng kể :)) . Biến $template sẽ bị ghi đè bởi tham số template từ URL
4. Không sử dụng Whitelisting với các file có thể include()

```
<?php
  if(file_exist('includes/' . ``` $_GET['file]; ``` . '.php')){
    include('includes/' . ``` $_GET['file]; ``` . '.php');
  }
?>
```
 Trường hợp này là có check nhưng không đáng kể. Có biết check xem file có tồn tại trước khi include thay vì cứ đẩy input vô tùm lum . Nhưng mà không có whitelist về các file có thể truy cập tùy theo ủy quyền .

5. Sử dụng đầu vào người dùng trong các hàm đọc file 
```
<?php
  $content = file_get_contents('content/' . $_GET['filename']);
  echo $content;
?>
```
 Hàm file_get_contents sẽ bị khai thác nếu như không filter đúng cách























