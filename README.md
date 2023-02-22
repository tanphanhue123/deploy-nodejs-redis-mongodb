+ Cài đặt Nginx ở môi trường ngoài Docker (dễ quản lý, dễ xài),cấu hình Nginx như 1 Reverse Proxy

+ Cài đặt Nginx
- sudo apt update
- sudo apt install nginx
- sudo service nginx status

+ Tiếp theo cài certbox
- sudo add-apt-repository ppa:certbot/certbot
- sudo apt-get update
- sudo apt-get install python-certbot-nginx

+ Xóa cấu hình các file cố định của Nginx
- sudo rm /etc/nginx/sites-available/default
- sudo rm /etc/nginx/sites-enabled/default

+ Tạo 1 file cấu hình cho domain của chúng ta (ở đây là tanphanhue.shop)
- sudo vi /etc/nginx/sites-available/tanphanhue.shop
- Pass nội dung này vào:

server {
        listen [::]:80;
        listen 80;
        
        # allow upload file with size upto 500MB
        client_max_body_size 500M;

        server_name tanphanhue.shop www.tanphanhue.shop;

        location / {
                proxy_pass http://localhost:3000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_set_header x-forwarded-for $remote_addr;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_cache_bypass $http_upgrade;
        }
}

+ Tiếp theo cần tạo Symbolic Link tới folder /etc/nginx/sites-enabled ( để web của mình có thể active )
- sudo ln -s /etc/nginx/sites-available/tanphanhue.shop /etc/nginx/sites-enabled/

+ Kiểm tra cấu hình Nginx ( sudo nginx -t )

+ Khởi động lại Nginx Service 
- sudo service nginx restart

+ Lấy HTTPS
- sudo certbot --nginx -d tanphanhue.shop -d www.tanphanhue.shop

