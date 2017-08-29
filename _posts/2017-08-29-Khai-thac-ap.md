---
title: Khai thác AP!
tags: [AP, Exploit AP]
---
Bài viết trình bày về buổi semina về khai thác AP của team ngày 25/08/2017



---
# 25/08/2017 - Khai thác AP
>Thành phần:  
>- Khánh Saker  
>- Quốc Anh  
>- Trần Trung  

## Giới thiệu về PentestBox
Truy cập vào trang: http://tools.pentestbox.org để tham khảo các tools được PentestBox hỗ trợ. Trong đó Pentesbox chia thành các nhóm tools theo từng chủ đề nhất định.  
1. Web Vulnerability Scanners  
2. Web Applications Proxies  
3. CMS Vulnerability Scanners  
4. Web Crawlers  
5. Information Gathering  
6. Exploitation Tools  
7. Password Attacks  
8. Android Security  
9. Reverse Engineering    
10. Stress Testing  
11. Sniffing  
12. Forensic Tools  
13. Wireless Attacks  
14. Text Editors  
15. Linux Utilities  
16. Browser  
Có thể thêm các tools trên Git bằng cách tham khảo [link][1].

> Bài hướng dẫn sử dụng nhóm công cụ **Aircrack**  
> - Author: [Khánh Saker](khanhsaker97.github.io)  
## Tấn công Wifi sử dụng mã hoá WPA
*Giới thiệu về WEP, WPA/WPA2*  
WEP, WPA/WPA2 là những chuẩn bảo mật phổ biến để bảo vệ mạng wifi, bảo đảm an toàn cho kết nối không dây. WEP là một giao thức bảo mật cũ với nhiều hạn chế về bảo mật và hiện tại đã được thay thế bởi 2 chuẩn WPA/WPA2 (WiFi Protected Access). WEP viết tắt của Wired Equivalent Privacy (Riêng tư tương tự mạng dây), WPA là Wireless Protected Area (vùng bảo vệ không dây). WPA2 là phiên bản thứ hai của chuẩn WPA.

*Giới thiệu Aircrack-ng*  
**Aircrack-ng** là bộ công cụ mạnh mẽ trong **Kali Linux** phục vụ cho quá trình đánh giá bảo mật mạng Wifi. Bộ công cụ này gồm nhiều công cụ với các chức năng như:
- `airmon-ng` – Dùng để chuyển card Wireless sang chế độ **monitor** (chế độ theo dõi và thu thập tín hiệu Wifi).
- `airodump-ng` – dùng để phát hiện các điểm phát sóng và bắt các gói tin 802.11.
- `aireplay-ng` – tạo ra dòng tín hiệu tác động đến mạng.
- `aircrack-ng` – tìm ra mã khóa WEP.

*Tấn công mật khẩu bằng Dictionary Attack*  
1. Công cụ tạo từ điển: `Crunch <min> <max> -o wordlist` hoặc lấy wordlist trên Google.<br/>  
2. Kiểm tra tên card Wireless đang sử dụng bằng lệnh `iwconfig`, thông thường là card wlan0. Nếu card wireless chưa được bật (không thể kết nối wifi) thì có thể bật bằng lệnh `ifconfig wlan0 up`.<br/>  
3. Chuyển card mạng Wifi sang chế độ **monitor** (chế độ theo dõi toàn bộ các tín hiệu trong mạng) bằng `airmon-ng start wlan0`.<br/>  
4. Xác định mạng wifi mục tiêu và sử dụng airodump để bắt gói tin và chỉ theo dõi duy nhất mạng mục tiêu đó:<br/>  
`airodump-ng -c [channel] -w [tập tin] --bssid [BSSID của mạng] wlan0mon`  
Ví dụ: `airodump-ng -c 9 -w wifi-sniff --bssid C4:6E:1F:F6:34:B8 wlan0mon`.  
Trong đó:+ quan sát trường *CH* để xác định *Channel* của điểm phát sóng  
+ `-w[tập tin]`: xác định đường dẫn để lưu tập tin bắt được (định dạng .cap)  
+ `bssid`: xem trường BSSID (địa chỉ MAC của access point)<br/>  
5. Thu thập gói tin bắt tay **WPA handshake** (bắt tay 4 bước) trong quá trình đăng nhập để dựa vào đó dò tìm mật khẩu.  
có 2 cách:  
+ Chờ người dùng nào đó đăng nhập vào wifi đang theo dõi.  
+ Sử dụng aireplay để tạo tín hiệu deauth (kích các người dùng đang sử dụng mạng thoát ra và đăng nhập lại liên tục.  
Cú pháp:  
    `aireplay-ng --deauth [số lệnh deauth] -a [BSSID của mạng] wlan0mon`  
    Ví dụ: `aireplay-ng --deauth 5 -a C4:6E:1F:2D:D6:B8 wlan0mon`  
    (có thể thay thế -deauth thành -0, khi muốn gửi  không giới hạn lệnh deauth có thể đặt thông số là 0)<br/>  
6. Thực hiện chờ hoặc dùng **aireplay** như bước 5 đến khi nhận được gói tin **W*PA handshake** của mạng mục tiêu tương ứng, ta dừng quá trình bắt gói tin ( Ctrl + C) và tiến hành dò tìm mật khẩu dựa vào file .cap đã bắt được.<br/>  
7. Tập tin mật khẩu **Wordlist** có thể Google, nếu biết trước một số điều kiện của mật khẩu có thể sử dụng công cụ **Crunch** để tạo danh sách mật khẩu nhanh.<br/>  
8. Sử dụng **Aircrack-ng** với **Wordlist** bên trên tiến hành tấn công Dictionary  

### Các trường hợp cần đo thời gian
1. 5 ký tự toàn là số 0 -->9: wordlist5Num: Thời gian??
2. 6 ký tự toàn là số 0 -->9: Wordlist6Num
3. 7 ký tự toàn là số 0 -->9: Wordlist7Num
4. 8 ký tự toàn là số 0 -->9: Wordlist8Num
5. 5 ký tự trong đó có 1 ký tự là chữ cái: wordlistChar

## Các trường hợp tấn công Wifi
[x] Bẻ khoá (Brutfore - Dictionary Attack)  
[ ] Dựng 1 wifi ảo lên. Deface cái gốc.  
	- Fluxion: Dự kiến ngày 01/09/2017 sẽ thực hiện  

## Khai thác thông tin trong mạng Wifi lần gặp tới
### Công cụ
[ ]Cain&Abel  
[ ]WireShark--> Lưu 1 file --> Networkminner  
[ ]Burpsuite  

### Kịch bản
- 1 máy làm máy chủ: EasyPHP + Dvwa
- 1 máy làm người dùng bình thường.
- 1 máy dùng công cụ để do thám.
- Detect xem trong mạng có sniffer không?  

### Tìm hiểu thêm về Lưu trữ mật khẩu
- Hàm Hash là gì?
- Cơ chế lưu trữ mật khẩu trên web?  
## Tham khảo
- [Awesome web Hacking][1]
[1]: https://github.com/infoslack/awesome-web-hacking "Awesome web Hacking"