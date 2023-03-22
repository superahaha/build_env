# Apache配置http自动跳转https

先在httpd.conf打开80端口转发开关

AllowOverride All

## 方法一
<IfModule mod_rewrite.c>
    RewriteEngine on
    RewriteCond %{SERVER_PORT} !^443$
    RewriteRule ^(.*)?$ https://%{SERVER_NAME}/$1 [R=301,L]  # 整站跳转
</IfModule>


## 方法二
RewriteEngine on
RewriteCond   %{HTTPS} !=on
RewriteRule   ^(.*)  https://%{SERVER_NAME}$1 [L,R]


## 方法三
RewriteEngine on 
RewriteCond %{SERVER_PORT} !^443$ 
RewriteCond %{REQUEST_URI} !^/tz.php 
RewriteRule (.*) https://%{SERVER_NAME}/$1 [R]


## 方法四
RewriteEngine On 
RewriteBase / 
RewriteCond %{SERVER_PORT} 80 
RewriteRule ^(.*)$ <a href="https://www.xxx.com/" target="_blank">https://www.xxx.com/</a>$1 [R=301,L]


## 方法五
RewriteEngine on 
RewriteBase /yourfolder 
RewriteCond %{SERVER_PORT} !^443$ 
#RewriteRule ^(.*)?$ https://%{SERVER_NAME}/$1 [R=301,L] 
RewriteRule ^.*$ https://%{SERVER_NAME}%{REQUEST_URI} [R=301,L]
#以上至针对某个目录跳转， yourfolder就是目录名


## 方法六
redirect 301 /[你的网页] https://[你的主机]/[网页]
#至针对某个网页跳转


## 指定站点跳转
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteCond %{HTTP_HOST} ^example.com [NC,OR]
RewriteCond %{HTTP_HOST} ^www.example.com [NC]
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R,L]
