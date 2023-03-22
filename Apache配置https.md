# 先安装mod_ssl，yum install mod_ssl

1. 生成根证书的私钥*
openssl genrsa -out server.key 2048
openssl genrsa -out zentao.key 2048
2. 生成服务器证书的申请文件*
openssl req -new -key server.key -out server.csr
openssl req -new -key zentao.key -out zentao.csr
3. 生成服务器证书*
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
openssl x509 -req -days 3650 -in zentao.csr -signkey zentao.key -out zentao.crt



Listen 443 https

SSLPassPhraseDialog exec:/usr/libexec/httpd-ssl-pass-dialog

SSLSessionCache         shmcb:/run/httpd/sslcache(512000)
SSLSessionCacheTimeout  300

SSLRandomSeed startup file:/dev/urandom  256
SSLRandomSeed connect builtin

SSLCryptoDevice builtin

<VirtualHost _default_:443>
DocumentRoot "/var/www/html"
ServerName 18.162.170.147:443

ErrorLog logs/ssl_error_log
TransferLog logs/ssl_access_log
LogLevel warn

SSLEngine on
SSLProtocol all -SSLv2 -SSLv3
SSLCipherSuite HIGH:3DES:!aNULL:!MD5:!SEED:!IDEA
SSLCertificateFile /etc/httpd/cert/zentao.crt
SSLCertificateKeyFile /etc/httpd/cert/zentao.key

<Files ~ "\.(cgi|shtml|phtml|php3?)$">
    SSLOptions +StdEnvVars
</Files>
<Directory "/var/www/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>

BrowserMatch "MSIE [2-5]" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0

CustomLog logs/ssl_request_log \
          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"

</VirtualHost>


# 监听端口直接走https
Listen 60080 https

<VirtualHost *:60080>

    DocumentRoot "/var/www/zentaopms/www"
    ServerName 18.162.170.147:60080

    <Directory "/var/www/zentaopms/www">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    SSLEngine on
    SSLProtocol all -SSLv2 -SSLv3
    SSLCipherSuite HIGH:3DES:!aNULL:!MD5:!SEED:!IDEA
    SSLCertificateFile /etc/httpd/cert/zentao.crt
    SSLCertificateKeyFile /etc/httpd/cert/zentao.key

</VirtualHost>


# 配置端口转发
# 修改httpd.conf文件，把下三个模块前面的#去掉
 #LoadModule proxy_module modules/mod_proxy.so
 #LoadModule proxy_connect_module modules/mod_proxy_connect.so
 #LoadModule proxy_http_module modules/mod_proxy_http.so

# 添加转发配置
<VirtualHost *:80>
    ProxyPreserveHost on
    ServerAdmin root@xxx.com
    ServerName xxx.com
    ServerAlias www.xxx.com

    ProxyPass / https://127.0.0.1:8080/
    ProxyPassReverse / https://127.0.0.1:8080

    # 重定向
    #RewriteEngine On 
    #RewriteCond %{HTTP_HOST} ^自己的域名$ 
    #RewriteRule ^/(.*)$ https://自己的域名/$1 [L,R=301]

</VirtualHost>

