---
title: Nginx的编译安装
layout: page
category: nginx
date: 2016-06-20
modifiedOn: 2016-06-20
---

这里主要讲解CentOS下 Nginx的编译安装，其他OS的安装方式类似

## 编译安装

下载地址: http://nginx.org/
安装准备: nginx依赖于pcre库,要先安装pcre

```javascript
yum install pcre pcre\-devel
yum \-y install gcc gcc-c++ autoconf automake make
yum -y install zlib zlib-devel openssl openssl--devel pcre pcre-devel 
cd /usr/local/src/
wget http:\/\/nginx.org/download/nginx-1.4.2.tar.gz
tar zxvf nginx-1.4.2.tar.gz
cd nginx-1.4.2
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module
make && make install
```

## 常见第三方模块

Nginx提供的常用的第三方模块

- **http_stub_status_module** 
- **http_ssl_module**
- **http_gzip_static_module**
- **http_consistent_hash**
- **http_upstream_module**
- **http_proxy_module**
- **http_php_memcached**

## Nginx启动、关闭、重载

Nginx的启动、关闭、重载命令

- **启动命令:** /usr/local/nginx/sbin/nginx
- **关闭命令:** /usr/local/nginx/sbin/nginx -s stop
- **重启命令:** /usr/local/nginx/sbin/nginx -s restart
- **配置文件重载:** /usr/local/nginx/sbin/nginx -s reload
- **检查配置命令:** /usr/local/nginx/sbin/nginx -t











