---
title: 在Hestia CP中添加thinkphp模版
date: 2024-01-23T11:06:31+08:00
tags:
  - ubuntu
  - hestia
  - vps
  - control panel
  - thinkphp
categories:
  - Tutorial
code:
  maxShownLines: 11
# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

# 环境
 ubuntu + hestia cp

# 为thinkphp 添加php-fpm模版
- 首先通过SSH链接到远程主机
  ```bash
  cd /usr/local/hestia/data/templates/web/nginx/php-fpm
  vim thinkphp.tpl
  ```
  
  ```nginx
  server {
    listen      %ip%:%web_port%;
    server_name %domain_idn% %alias_idn%;
    root        %docroot%/public;
    index       index.php index.html index.htm;
    access_log  /var/log/nginx/domains/%domain%.log combined;
    access_log  /var/log/nginx/domains/%domain%.bytes bytes;
    error_log   /var/log/nginx/domains/%domain%.error.log error;

    include %home%/%user%/conf/web/%domain%/nginx.forcessl.conf*;

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }

    # 隐藏特定文件
    location ~* /(README\.md|composer\.json|composer\.lock|\.git) {
        deny all;
        return 404;
    }

    # 静态文件处理，防止对静态文件执行 PHP
    location ~* \.(jpg|jpeg|gif|png|css|js|ico|webp|tiff|ttf|svg|eot|woff|otf|woff2|mp4|ogg|ogv|webm)$ {
        expires 7d;
        access_log off;
        add_header Cache-Control "public, no-transform";
    }

    # PHP 文件处理
    location ~ \.php$ {
        try_files $uri =404;
        include /etc/nginx/fastcgi_params;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass %backend_lsnr%;

        include %home%/%user%/conf/web/%domain%/nginx.fastcgi_cache.conf*;

        # 其他 fastcgi 相关参数...
    }

    # URL 重写规则，适用于 ThinkPHP 6
    location / {
        try_files $uri $uri/ /index.php$is_args$args;
        # try_files $uri /index.php$is_args$args; crmeb自带
        if (!-e $request_filename){
            rewrite  ^(.*)$  /index.php?s=$1  last;   break;
        }
    }

      # 其他配置...
      include %home%/%user%/conf/web/%domain%/nginx.conf_*;
  }

  ```
  
  ```
  #=========================================================================#
# Default Web Domain Template                                             #
# DO NOT MODIFY THIS FILE! CHANGES WILL BE LOST WHEN REBUILDING DOMAINS   #
# https://hestiacp.com/docs/server-administration/web-templates.html      #
#=========================================================================#

server {
listen      %ip%:%web_ssl_port% ssl;
server_name %domain_idn% %alias_idn%;
root        %sdocroot%/public;  # 确保指向 public 目录
index       index.php index.html index.htm;
access_log  /var/log/nginx/domains/%domain%.log combined;
access_log  /var/log/nginx/domains/%domain%.bytes bytes;
error_log   /var/log/nginx/domains/%domain%.error.log error;

    ssl_certificate     %ssl_pem%;
    ssl_certificate_key %ssl_key%;
    ssl_stapling        on;
    ssl_stapling_verify on;

    # TLS 1.3 0-RTT anti-replay
    if ($anti_replay = 307) { return 307 https://$host$request_uri; }
    if ($anti_replay = 425) { return 425; }

    include %home%/%user%/conf/web/%domain%/nginx.hsts.conf*;

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }

    # 隐藏敏感文件
    location ~ /(README\.md|composer\.json|composer\.lock|\.git) {
        deny all;
        return 404;
    }

    # 静态文件处理
    location ~* \.(jpg|jpeg|gif|png|css|js|ico|webp|tiff|ttf|svg|eot|woff|otf|woff2|mp4|ogg|ogv|webm)$ {
        expires 7d;
        access_log off;
    }

    # PHP 文件处理
    location ~ \.php$ {
        try_files $uri =404;
        include /etc/nginx/fastcgi_params;
        fastci_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass %backend_lsnr%;

        include %home%/%user%/conf/web/%domain%/nginx.fastcgi_cache.conf*;
    }

    # ThinkPHP 6 URL 重写规则
    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    include %home%/%user%/conf/web/%domain%/nginx.ssl.conf_*;
}

  ```


<!--more-->
