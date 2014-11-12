NGINX.
==========================
Nginx là một open source reverse proxy server cho HTTP, HTTPS, SMTP...protocol.

TUT này hướng dẫn bạn cấu hình một configuration file cho nginx.

1. Configuration File’s Structure.
-----------------
Nginx configuration file thường đặt tại ```/usr/local/nginx/conf```, ```/etc/nginx```, hoặc ```/usr/local/etc/nginx```.

Nginx configutation file bao gồm các modules, được điều khiển bởi các directives được quy định
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

2. Serving Static Content.
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
Mỗi lần nginx quyết định server xử lý một requests. nó sẽ kiểm tra URI được xác
định trong request's header phù hợp với các tham số của **location** directives,
được định nghĩa bên trong server block.

Location block này xác định tiền tố '/' đối hiếu với URI từ request. Nếu matching
request, URI sẽ được add thêm phần path được xác định trong root directives để hình
đường dẫn đến các files cần thiết nằm trong local file system. Nếu có nhiều
location block phù hợp với URI, nginx sẽ chọn location có prefix dài nhất.

Trong trường hợp trên, location block có prefix '/', ngắn nhất. Vì vậy, nó chỉ
được chọn khi không có location nào khác phù hợp với request.

 Tiếp theo add location block thứ 2:

```
location /images/ {
         root /data;
 }
```
Trong trường hợp trên, sẽ matching các request bắt đầu với **/images/**

Vậy kết quả **server** block của chúng ta sẽ gồm:
```
server {
    location / {
        root /data/www;
                }

    location /images/ {
        root /data;
              }
        }
```
Khi làm việc, server này sẽ listen trên port 80 và truy cập vào local machine tại
**http://localhost/.**. Khi đáp ứng một request với URIs bắt đầu với /images/. server
sẽ send các file trong thư mục **/data/images**.

Ví dụ: để đáp ứng **http://localhost/images/example.png** request, nginx sẽ send
file **example.png**. Nếu file này không tồn tại, sẽ xuất hiện lỗi 404. Các request như:
```
http://localhost/some/example.html
```
không bắt đầu với /images/sẽ được map tới file **/data/www/some/example.html**.

Để apply configuration mới, bạn cần reload:
```
nginx -s reload
```
Trong trường hợp nginx không làm việc, bạn có thể check log trong /var/log/nginx/...

3. Thiết lập một proxy server đơn giản.
--------------------------------
Nginx thường xuyên được sử dụng như một proxy server.

Chúng ta sẽ thực hiện config một proxy server đơn giản để làm ví dụ phân tích.
Server này sẽ phục vụ yêu cầu với các images trong thư mục local và send tất cả
các request tới một proxied server. Trong trường hợp này, cả hai server sẽ được định
nghĩa trên một nginx instance.

Đầu tiên, định nghĩa thêm một server block  trong configuration file:
```
server {
    listen 8080;
    root /data/up1;

    location / {
        }
}
```
Bạn đã định nghĩa xong một server listen trên port 8080, và map tất cả các request
tới thư mục /data/up1 trên local. Create thư mục đó và tạo một file index.html và
đặt vào đó.

Tiếp theo, định nghĩa một proxy server. Chúng ta sử dụng **proxy_pass** để khai
báo proxied server:
```
server {
    location / {
        proxy_pass http://localhost:8080;
        }
    location /images/ {
        root /data;
            }
}
```
Trong location thứ 2, để quy định các phần mở rộng của images, bạn có thể đổi lại
location như sau:
```
location ~ \.(gif|jpg|png)$ {
        root /data/images;
}
```
Khi đó, config cho proxy server như sau:
```
server {
    location / {
        proxy_pass http://localhost:8080/;
                }
    location ~ \.(gif|jpg|png)$ {
       root /data/images;
                 }
}
```
server này sẽ filter các request kết thúc với .gif, .jpg,.png và map chúng tới
**/data/images** và pass tất cả các request đó tới proxied server ở trên.

4. Một số lệnh hay dùng với nginx.
----------------------------------------
- nginx -s reload: load configuration file mới.
- nginx -s stop: stop nginx.
- nginx -s start: start nginx.
- nginx -s reopen: reopen log file
- ...

Link tham khảo:

http://nginx.org/en/docs/beginners_guide.html
