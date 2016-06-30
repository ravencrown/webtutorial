---
title: Nginx 虚拟主机配置
layout: page
category: nginx
date: 2016-06-30
modifiedOn: 2016-06-30
---

Nginx虚拟主机配置有三种方式：基于域名的虚拟主机配置、基于端口的虚拟主机配置、基于IP的虚拟主机配置

## 基于域名的虚拟主机配置
	
示例如下

```javascript

server {
    listen 80;  #监听端口
    server_name a.com; #监听域名
    location / {
        root /var/www/html;   #绝对路径定位
        index index.html;
    }
}

server {
    listen 80;
    server_name z.com;

    location / {
        root z.com;  #相对路径定位，相对于nginx根目录定位
		index index.html;
    }
    access_log logs/z.com.access.log main;    #访问日志设置
}

```

## 基于端口的虚拟主机配置

示例如下

```javascript
server {
    listen 8080;
    server_name z.com;

    location / {
            root html;
            index index.html;
    }
}
```


## 基于IP的虚拟主机配置

示例如下

```javascript
server {
    listen 80;
    server_name 192.168.1.204;

    location \/ {
        root /var/www/html;
        index index.html;
    }
}
```




