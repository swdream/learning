SSL and TLS: A Begginner's Guide.
========================================
SSL và TLS là hai secure protocol phổ biến trong internet security, đặc biệt là
sử dụng trong các trang web và servers chứa các dữ liệu nhạy cảm như password,
thông tin người dùng...
Bài viết có mục đích:
- Giải thích về **Secure Socket Layer** (SSL) và **Transpot Layer Security** (TLS).
- Cách thức chúng có thể được applied tới một web application.
- Các vấn đề cần thiết để tạo một secure link giữa server và client.

1. Giới thiệu chung.
------------------------------------
Hiện nay, các database driven applications như MySQL, Oracle... đang ngày càng được
sử dụng rộng rãi. Tính bảo mật và toàn vẹn của thông tin được đặt lên là nhiệm
vụ cấp thiết hàng đầu.

Một trong các cách thức hiệu quả nhất là sử dụng một communicaton protocol để mã hóa dữ liệu giữa
client và server. Và TLS và SSL là hai trong số các communication protocols phổ biến nhất.

2. Secure Socket Layer protocol (SSL).
----------------------------------------
SSL là secure communication protocol được chọn cho phần lớn các internet community.
Có một vài ứng dụng phổ biến của SSL như
