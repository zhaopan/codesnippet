# Nginx Configs

* version: nginx-1.12.2

## nginx/conf.d/xx.conf

```text
server {
    listen       80;
    server_name  git.xx.com;
    access_log logs/git.xx.com.log;
    location / {
        #一定要注意这里是docker容器的内网地址+端口,以"/"结尾,不然会报错。
        proxy_pass http://172.18.0.3:3000/;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        client_max_body_size       10m;
        client_body_buffer_size    128k;
        proxy_connect_timeout      70;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        #proxy_send_lowat           12000;
        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        #proxy_temp_file_write_size 64k;
    }
}
```

## error

```bash
# eg:
# centos7 nginx=>nginx
# errorStr => failed (13: Permission denied) while connecting to upstream
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
setsebool -P httpd_can_network_connect 1
```

