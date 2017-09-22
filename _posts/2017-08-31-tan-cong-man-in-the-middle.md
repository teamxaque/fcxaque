---
title: Tấn công giả mạo AP và thử nghiệm MITM trong mạng LAN!
tags: [AP, Exploit AP, MITM Attack, Cain&Abel, DVWA]
excerpt_separator: <!--more-->
---
Bài viết note lại buổi semina về tấn công giả mạo AP và thử nghiệm MITM trong mạng LAN với Cain&Abel ngày 31/08/2017
<!--more-->

---
>Thành phần:  
>- Khánh Saker  
>- Quốc Anh  
>- Trần Trung  
   
---
# Tấn công giả mạo AP với Wifi Slax - Linset

Tác giả: Trung Trần

1. Khởi động Linset

2. Chọn Card wifi
	- Hệ thống sẽ liệt kê những điểm AP gần đó
	- Lựa chọn 1 điểm phát AP để tấn công
3. bước tiếp theo....

---
# Tấn công giả mạo AP với Fluxion
Tác giả: [Khánh Saker]()



---
# Hướng dẫn tạo Server ảo + DVWA thử nghiệm do thám trong mạng

## Công cụ:
1. [EasyPHP](www.easyphp.org/) hoặc công cụ cho phép tạo Server ảo ([XAMPP](https://www.apachefriends.org/), [AppServer](https://www.appserv.org/), [WampServer](www.wampserver.com/en/)....)
2. [DVWA](http://www.dvwa.co.uk/)

## Các bước thực hiện:
1. Cấu hình server
	- Configuration --> Apache
	- Sửa địa chỉ IP thành **IP của máy làm Server** và cổng kết nối (ví dụ: `192.168.1.104:8080`)
	- Restart lại Apache
2. Copy thư mục **DVWA** vào thư mục **www** của **EasyPHP**
3. Bật trình duyệt vào quản lý của EasyPHP: `http://192.168.1.104:8080/home/index.php`
4. Mở **PHPMyAdmin** để tạo CSDL: **DVWA**
5. Mở trình duyệt truy cập --> Done!

---
# Dùng Cain để tấn công MITM trong mạng
Tác giả: [Khánh Saker](https://khanhsaker97.github.io)
## Công cụ: 
- Cain

## Kịch bản tấn công:
- Một máy làm người dùng thường, truy cập vào website DVWA
- Một máy sử dụng Cain để Sniff password trong mạng LAN

## Các bước thực hiện:
1. Khởi động Cain
2. Configure --> Chọn card mạng
3. Nhấn biểu tượng card mạng. Chuyển sang thẻ Sniffer
4. Tại mục Host. Chọn dấu cộng xanh để thêm host. Có thể chọn **Resolve the host name** để xem tên máy
5. Tại mục APR. Chọn các host cần lắng nghe và thưởng thức thành quả.

---
# Kết quả của buổi semina

1. Trung:
2. Khánh:
- Biết các thao tác cơ bản sử dụng Cain để tiến hành nghe lén.
- Biết cấu hình xampp để máy khác vào được web server của mình trong mạng Lan
3. Quốc Anh:
- Nắm được mô hình của việc quản lý các trang web, khả năng truy cập của các user, tạo database của web
- Tìm cách phát hiện sniff.

---
# Chủ đề của tuần tới:
1. Liên quan tới khai thác AP
	Kịch bản: Tấn công MITM với AP ảo
	- 1 máy dựng server --> truy cập AP ảo
	- 1 máy Linset tạo AP ảo,
	- 1 máy làm nạn nhân
	Tìm hiểu: 
	- Cơ chế phát hiện mật khẩu đúng
	- Sửa mã nguồn trang đăng nhập (tìm được tập tin này...)
	- Cơ chế tính toán phần trăm thành công
	- So sánh **Fluxion** với **Linset**
	
2. Tìm hiểu lỗ hổng [MS08-070](https://www.rapid7.com/db/modules/exploit/windows/smb/ms08_067_netapi), [CVE-2017-0199](https://github.com/bhdresh/CVE-2017-0199). (Cài winXP)
3. Sử dụng Metasploit trong [PentestBox](https://pentestbox.org)
