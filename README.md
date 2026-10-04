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
