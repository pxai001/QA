### Mac

1. 安装

   ```shell
   sudo brew install nginx
   ```

2. 启动

   ```shell
   sudo nginx
   ```

3. 配置

   ```nginx
    server {
           listen       80;
           server_name  localhost;
        
           location / {
              proxy_pass   http://127.0.0.1:3000;
           }
   
           location /auth{
             proxy_pass   http://127.0.0.1:8001;
           }    
   
           location /data {
             proxy_pass   http://127.0.0.1:8002;
           }
    }
   ```