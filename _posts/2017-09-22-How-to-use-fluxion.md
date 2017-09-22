---
title: Tấn công mạng không dây sử dụng Fluxion
tags: [AP, Exploit AP, Fluxion]
excerpt_separator: <!--more-->
---
Mục đích của công cụ này là tạo ra một AP giả giống với AP thật. Sau đó dùng công cụ mdk3 để gửi các gói tin de-authentication, khiến người sử dụng không thể kết nối với AP thật
<!--more-->
# Giới thiệu công cụ

Công cụ Wifi mang tên Fluxion được xem đây là phiên bản làm lại của Linset bởi Vk496 với ít lỗi và nhiều tính năng hơn, có thể tương thích phiên bản mới nhất của Kali Linux (Rolling) .

Ý tưởng của công cụ này không có gì mới. Mục đích của công cụ này là tạo ra một AP giả giống với AP thật. Sau đó dùng công cụ mdk3 để gửi các gói tin de-authentication, khiến người sử dụng không thể kết nối với AP thật. Nếu người dùng kết nối tới AP giả mạo, tất cả lưu lượng mạng của họ sẽ bị chuyển tới trang đăng nhập. Trang đăng nhập này bao gồm tên AP, nếu người dùng nhập mật khẩu của AP thật, công cụ hoàn tất việc lấy mật khẩu.

Fluxion script được tạo ra rất thích hợp cho các nhà nghiên cứu bảo mật an ninh kiểm tra các mạng an ninh của họ bằng cách hack Wifi dựa trên các chương trình như Aircrack-ng, mdk3, hostapd v..v.. Công cụ này sử dụng kỹ thuật MITM tấn công các lớp bảo mật WPA/WPA2, mà không cần sử dụng các phần mềm có tính năng Bruteforce / Dictionary mất nhiều thời gian hơn.

Dowload [Fluxion](https://github.com/wi-fi-analyzer/fluxion): 
	```code
	- Dùng lệnh Git để Dowload: git clone https://github.com/wi-fi-analyzer/fluxion.git

	- Dowload trực tiếp: vào trang web trên chọn phần tìm đến phần "Clone or dowload" Chọn dowload zip.
	```

===
# Cài đặt:

- Chạy file fluxion.sh bằng lệnh terminal: ``./fluxion.sh``
- Nếu thấy thiếu các gói để cài đặt: thì vào file **install** rồi chạy file **install.py** bằng lệnh ``./install.py``

## Ngắn gọn các bước thực hiện:
1. Chương trình yêu cầu chọn card mạng để tiến hành
2. Quét tất cả các mạng nằm trong vùng phủ sóng.
3. Lựa chọn AP cần tấn công và tiến hành bắt gói tin handshake.
4. Mở giao diện WEB.
5. Tạo một FakeAP (Fake Access Point) để giả lập điểm truy cập gốc (original access point).
6. Chạy một tiến trình MDK3 để làm người dùng trong mạng (mục tiêu) bị mất xác thực với AP thật và đẩy người dùng ra khỏi AP.
7. Thực hiện tạo một DNS server fake để bắt các gói tin DNS về host.
8. Chạy một trang đăng nhập wifi giả để người dùng điền thông tin đăng nhập.
9. Mỗi lần dữ liệu được gửi lên thì thực hiện kiểm tra handshake với gói tin bắt ở 3.
10. Khi nào có mật khẩu chính xác được kiểm chứng, kết thúc tấn công.

## Các bước tiến hành
Xem thêm tại: ``https://www.youtube.com/watch?v=4ggZTG3rqXA``

===
# Tuỳ biến công cụ
## Chỉnh sửa tên AP: 
**Mục đích**: Cho phép tuỳ biến tên AP cho một số trường hợp tấn công. Ví dụ, khi cần tấn công ở môi trường công cộng có thể đổi tên thành "Free-wifi" hoặc một tên AP của các quán cafe, Internet...

**File cần sửa: Fluxion.sh**
- Tìm đến phần "**Create different settings required for the script**"
(Dùng Ctrl + F để tìm kiếm cho nhanh)
- Trong phần đó tìm phần "**Config HostAPD**" ngay đầu và sửa phần
`"ssid=$Host_SSID"` thành `ssid="Tên wifi"`.
- Bây giờ fake AP nào thì AP được tạo ra cũng có tên mặc định là "**Tên wifi**" ở bên trên.

## Chỉnh sửa giao diện web:
**Mục đích**: Nhằm tạo ra trang đăng nhập giống với mục tiêu hơn. Nhiều tổ chức, công ty áp dụng chế độ  đăng nhập web đối với các cá nhân, nhân viên trong tổ chức. Do đó, việc fake trang đăng nhập này cũng là một trong các yếu tố dễ dàng đánh lừa người dùng hơn.
**Các bước thực hiện**
- Vào thư mực **sites** trong đó có các file cấu hình trang web
- Chọn một thư mục bất kỳ để sửa. Có thể clone thư mục để tạo mới một trang đăng nhập khác. (Lúc sau tấn công phải chọn giao diện web đúng với thư mục đó. Có 44 giao diện web tương ứng với các thư mục trong phần site, tên giao diện web gần giống với tên thư mục)
- Về cơ bản là trong mỗi thư mục sẽ có 3 file chính là ``index.html + final.html + error.html``. Những file khác để xử lý không cần để ý nhiều
- Trong file **index.html** tìm thẻ ``<input type="password">`` sẽ có 1 thuộc tính ``name = "key1"``. Đây là cái khóa để trang web lấy dữ liệu và kết nối tới file fluxion.sh để lưu dữ liệu.
===
# Tham khảo
[Tấn công giả mạo AP và thử nghiệm MITM trong mạng LAN!](https://teamxaque.github.io/2017/08/31/tan-cong-man-in-the-middle.html)

[Hướng dẫn khai thác AP](https://teamxaque.github.io/2017/08/29/Khai-thac-ap.html)