# 利用docker搭建php环境

## 先决条件

电脑上安装docker与docker-compose

## 创建并进入php-docker目录

`mkdir php-docker && cd php-docker`

### 一、创建docker-compose.yml文件

`vim docker-compose.yml`

```
version: '2'

services:
    web:
        image: nginx:latest
        ports:
            - "8080:80"
        volumes:
            - ./code:/code
            - ./site.conf:/etc/nginx/conf.d/default.conf
        networks:
            - code-network
    php:
        image: php:fpm
        volumes:
            - ./code:/code
        networks:
            - code-network

networks:
    code-network:
        driver: bridge
```
### 二、创建site.conf文件用来改变nginx配置

`vim site.conf`
```
server {
    listen 80;
    index index.php index.html;
    server_name localhost;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /code;

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```

### 三、创建code目录,并创建index.php文件

`mkdir code`

```
<?php
echo 'hello world';
```

### 四、运行并打开浏览器查看

`docker-compose up -d`

localhost:8080

### 五、开始coding

#### 未完，待续......