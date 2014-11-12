Nginx bao gồm các modules, được điều khiển bởi các directives được quy định
trong file cấu hình. Derectives được chia thành các simple directives và các
block directives.

Một simple directive bao gồm tên và các tham số được phân biệt bởi các spaces
và kết thúc với **;**.

Một block directive tương tự cấu trúc của một simple directives, nhưng thay
vì kết thúc bởi ; thì nó giới hạn bởi { }.

Một block directive có thể bao gồm một hoặc nhiều **context**, là một directive
bên trong { }. Có các context chính:
- event.
- http.
- server.
- location.

cấu trúc: event > http> server> location.

**#** hiển thị một comment line.

Serving Static Content
---------------------
Một nhiệm vụ quan trọng của web server là phục vụ các files( như images và html file).

Phụ thuộc và request, các files sẽ được phục vụ từ các thư mục local khác nhau:
- /data/www ( chứa cá html file).
- /data/images chứa các images.

Điều này sẽ yêu cầu bạn khai báo trong config file và thiết lập một server block
bên trong http block. trong server block có hai location block.

Đầu tiên, tạo /data/www chứa các html files và /data/images directory chứa các images file.

File cấu hình sẽ bao gồm một http block chứa server block.
```
http {
        server {
                }
}
```
Server block bao gồm listen port và server name.

Add các location block:
```
location / {
        root /data/www;
}
```
và:
```
location /images/ {
         root /data;
 }
```
Mỗi lần nginx quyết định server xử lý một requests. nó sẽ kiểm tra URI được xác
định trong request's header phù hợp với các tham số của **location** directives,
được định nghĩa bên trong server block.

Location block này xác định tiền tố '/' đối hiếu với URI từ request. Nếu matching request, URI sẽ được
