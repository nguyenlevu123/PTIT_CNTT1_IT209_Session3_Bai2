<<<<<<< HEAD
# Cấu hình trang lỗi tùy chỉnh (Custom Error Page 404) trên Nginx

## 1. Mục tiêu & Bối cảnh kỹ thuật
- **Mục tiêu**: Cấu hình dịch vụ Web Server Nginx để phục vụ một trang lỗi 404 (Not Found) tự thiết kế thay cho trang báo lỗi mặc định của hệ thống.
- **Yêu cầu bảo mật**: Sử dụng chỉ thị `internal` của Nginx để ngăn chặn người dùng truy cập trực tiếp vào tệp nguồn `404.html` từ internet.
- **Môi trường**: Linux Ubuntu Server, Nginx Web Server.

## 2. Các bước thực hiện chi tiết

### Bước 1: Tạo thư mục web root và tệp tin `404.html`
Sử dụng lệnh sau để tạo thư mục chứa mã nguồn web và tệp giao diện lỗi:
```bash
sudo mkdir -p /var/www/my-web/html
sudo nano /var/www/my-web/html/404.html
```
*- Giải thích các cờ:*
- `-p`: Cờ `--parents` hoặc `-p` của lệnh `mkdir` cho phép tạo toàn bộ cây thư mục cha nếu chưa tồn tại mà không báo lỗi.

### Bước 2: Cấu hình Server Block Nginx
Tạo và chỉnh sửa tệp cấu hình Server Block:
```bash
sudo nano /etc/nginx/sites-available/my-web.conf
```
Nội dung cấu hình bao gồm việc chỉ định `error_page 404 /404.html;` và block `location = /404.html` chứa chỉ thị `internal`.

### Bước 3: Kích hoạt cấu hình và kiểm tra
Kích hoạt site bằng cách tạo symbolic link và kiểm tra lỗi cú pháp:
```bash
sudo ln -s /etc/nginx/sites-available/my-web.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
*- Giải thích các lệnh:*
- `ln -s`: Tạo một liên kết mềm (symbolic link) từ thư mục `sites-available` sang `sites-enabled`.
- `nginx -t`: Kiểm tra tính hợp lệ của cú pháp file cấu hình Nginx trước khi apply.
- `systemctl reload nginx`: Tải lại cấu hình Nginx mà không làm gián đoạn các kết nối hiện tại.

## 3. Kiểm tra & Xác thực kết quả
Thực hiện các lệnh `curl` để kiểm tra phản hồi từ server:

```bash
curl -I http://127.0.0.1/invalid-path-demo
curl -I http://127.0.0.1/404.html
```

![Ảnh chụp terminal kiểm tra Nginx 404](nginx_test_result.png)

## 4. Kết luận & Best Practices bảo mật vận hành
- **Bảo mật**: Việc sử dụng `internal` giúp chặn tuyệt đối việc người dùng bên ngoài duyệt trực tiếp file `404.html`, tránh lộ thông tin thiết kế giao diện hệ thống hoặc bị lợi dụng chiếm dụng tài nguyên.
- **Vận hành**: Luôn luôn chạy lệnh `sudo nginx -t` trước khi thực hiện `reload` hoặc `restart` dịch vụ Nginx để tránh sập hệ thống web do lỗi cú pháp cấu hình.
=======
Session 03 - Bài 2: Cấu hình trang lỗi tùy chỉnh (Custom Error Page 404 & internal directive)
1. Mục tiêu
Cấu hình Nginx phục vụ trang báo lỗi 404.html tự thiết kế thay cho giao diện mặc định.
Sử dụng chỉ thị internal của Nginx để ngăn người dùng truy cập trực tiếp tệp 404.html qua URL.
2. Các bước thực hiện
Bước 1: Tạo tệp 404.html trong web root /var/www/my-web/html/
sudo mkdir -p /var/www/my-web/html
sudo cp 404.html /var/www/my-web/html/
Bước 2: Cấu hình Nginx Server Block
Chỉnh sửa tệp /etc/nginx/sites-available/my-web.conf:

server {
    listen 80;
    server_name _;
    root /var/www/my-web/html;
    index index.html;

    error_page 404 /404.html;

    location = /404.html {
        root /var/www/my-web/html;
        internal;
    }

    location / {
        try_files $uri $uri/ =404;
    }
}
Bước 3: Reload Nginx
sudo nginx -t
sudo systemctl reload nginx
3. Kết quả kiểm tra
Test 1: Truy cập đường dẫn không tồn tại: curl -I http://<IP>/invalid-path-demo

Mã trả về: HTTP/1.1 404 Not Found
Nội dung: Trang HTML tùy biến của 404.html.
Test 2: Truy cập trực tiếp file lỗi: curl -I http://<IP>/404.html

Mã trả về: HTTP/1.1 404 Not Found (Do chỉ thị internal ngăn chặn truy cập trực tiếp từ phía client).
>>>>>>> 7dca23ba3cdfab6713acdd26878f184b5a9273fc
