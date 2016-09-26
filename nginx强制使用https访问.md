- 基于nginx搭建了一个https访问的虚拟主机，监听的域名是test.com，但是很多用户不清楚https和http的区别，会很容易敲成http://test.com，这时会报出404错误，
所以我需要做基于test.com域名的http向https的强制跳转
我总结了三种方式，跟大家共享一下

###一.安装https并配置nginx支持https

#####1.安装https协议
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt
```
#####2.配置https
```
server {
        listen       80;
        listen       443;
        ssl on;
        server_name  47.88.193.69 www.divcloud.net divcloud.net;
        ssl_certificate /etc/nginx/ssl/nginx.crt; 
        ssl_certificate_key /etc/nginx/ssl/nginx.key;
```
###二强制https转http，有3种使用方案
#####1.nginx的497状态码

error code 497

497 - normal request was sent to HTTPS  

解释：当此虚拟站点只允许https访问时，当用http访问时nginx会报出497错误码
 
思路

利用error_page命令将497状态码的链接重定向到https://test.com这个域名上
 
配置

```
  server {
        listen       80;
        listen       443;
        ssl on;
        server_name  47.88.193.69 www.divcloud.net divcloud.net;
        add_header X-Cache $upstream_cache_status;
        ssl_certificate /etc/nginx/ssl/nginx.crt; 
        ssl_certificate_key /etc/nginx/ssl/nginx.key;

        #让http请求重定向到https请求   
        error_page 497  https://$host$uri?$args;  
```
#####nginx的rewrite方法
思路

这应该是大家最容易想到的方法，将所有的http请求通过rewrite重写到https上即可
 
配置
```
server {  
    listen  192.168.1.111:80;  
    server_name test.com;  
      
    rewrite ^(.*)$  https://$host$1 permanent;  
} 
```
#####index.html刷新网页
可以看到百度很巧妙的利用meta的刷新作用，将baidu.com跳转到www.baidu.com.因此我们可以基于http://test.com的虚拟主机路径下也写一个index.html，
内容就是http向https的跳转

