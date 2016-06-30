---
title: Location正则
layout: page
category: nginx
date: 2016-06-20
modifiedOn: 2016-06-20
---

## Location正则

```python
location / {
	root   /usr/local/nginx/html;
    index  index.html index.htm;
}
 
location ~ image {
   root /var/www/image;
   index index.html;
}
```

如果我们访问  http://xx.com/image/logo.png  
此时, “/” 与”/image/logo.png” 匹配  
同时,”image”正则 与”image/logo.png”也能匹配,谁发挥作用?  
正则表达式的成果将会使用.  
 
图片真正会访问 /var/www/image/logo.png  


```python
location / {
 	root	/usr/local/nginx/html;
    index	index.html index.htm;
}
 
location /foo {
    root	/var/www/html;
    index	index.html;
}
```

我们访问http://xxx.com/foo  
 对于uri “/foo”,   两个location的patt,都能匹配他们  
即 ‘/’能从左前缀匹配 ‘/foo’, ‘/foo’也能左前缀匹配’/foo’,  
此时, 真正访问 /var/www/html/index.html  
原因:’/foo’匹配的更长,因此使用之.;  

**location解析过程如下图**  

![location解析过程](https://camo.githubusercontent.com/7df924e86acaa7b2d44f709bc86aec92d64444c0/687474703a2f2f692e696d6775722e636f6d2f655336424476622e706e67)

## 参考链接

- Nginx官网，[Location Syntax](http://nginx.org/en/docs/http/ngx_http_core_module.html#location)













