title: LNMP1.0配置php与jsp共存
date: 2013-12-16 11:29:59
updated: 2013-12-16 11:29:59
categories:
  - Linux
tags:
  - linux
  - php
---

最近更换了一个vps，新的vps当然需要重新配置环境了，之前用的vps死活弄不好php与jsp共存，一直是我心中的痛...  
换了新的继续试一试能不能弄好，没想到一试真的配置好了，记录一下，以后没准还会用到。

安装lnmp之类的就不详细赘述了，都是很简单的操作，只简单说一下需要安装的有哪些东西，括号里是我本次安装的版本。
```
lnmp(1.0) /usr/local/nginx /usr/local/mysql /usr/local/php
jdk(7u45) /usr/local/jdk/jdk1.7.0_45
tomcat(7.0.47) /usr/local/tomcat7
```
## 注意事项
### 1.jdk
jdk安装之后需要配置`/etc/profile`  
`vi /etc/profile` 在文件的最后加入如下内容
```
JAVA_HOME="/usr/local/jdk/jdk1.7.0_45"
CLASS_PATH="$JAVA_HOME/lib:$FAVA_HOME/jre/lib"
PATH=".:$PATH:$JAVA_HOME/bin"
CATALINA_HOME="/usr/local/tomcat7"
export JAVA_HOME CATALINA_HOME
```
保存之后控制台运行`source /etc/profile`命令  
可运行`java -version`检测jdk是否安装成功
### 2.tomcat
tomcat安装后
```
cp -rf /usr/local/tomcat7/webapps/* /home/wwwroot/jsp/
vi /usr/local/tomcat/conf/server.xml
```
查找`appBase="webapps"`，修改为：`appBase="/home/wwwroot/jsp/"`,其中`/home/wwwroot/jsp/`为网站的根目录  
安装修改后，启动tomcat
`/usr/local/tomcat7/bin/startup.sh`  
`netstat -ntl` 看有没有8080端口
关闭tomcat  
`/usr/local/tomcat7/bin/shutdown.sh`

## nginx 与 tomcat 配置
`vi /usr/local/nginx/conf/nginx.conf`
```
user www www;

worker_processes 8;
#worker_processes 1;
#2013年11月29日16:57:22

error_log /home/wwwlogs/nginx_error.log crit;

pid /usr/local/nginx/logs/nginx.pid;

#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 51200;

events
{
  use epoll;
  worker_connections 51200;
}

http
{
  include mime.types;
  default_type application/octet-stream;

  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 300m;
  #client_max_body_size 50m;

  sendfile on;
  tcp_nopush on;

  keepalive_timeout 60;

  tcp_nodelay on;

  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 256k;

  proxy_connect_timeout 5;
  proxy_read_timeout 60;
  proxy_buffer_size 16k;
  proxy_buffers 4 32k;
  proxy_busy_buffers_size 64k;
  proxy_temp_file_write_size 128k;

  gzip on;
  gzip_min_length 1k;
  gzip_buffers 4 16k;
  gzip_http_version 1.0;
  gzip_comp_level 2;
  gzip_types text/plain application/x-javascript text/css application/xml;
  gzip_vary on;
  gzip_proxied expired no-cache no-store private auth;
  gzip_disable "MSIE [1-6].";

  #limit_zone crawler $binary_remote_addr 10m;

  server_tokens off;
  #log format
  log_format access '$remote_addr - $remote_user [$time_local] "$request" '
  '$status $body_bytes_sent "$http_referer" '
  '"$http_user_agent" $http_x_forwarded_for';

  upstream tomcat_server {
    server 127.0.0.1:8080;
  }

  server
  {
    listen 80;
    server_name localhost;
    index index.html index.htm index.php index.jsp default.jsp index.do default.do;
    root /home/wwwroot;

    if (-d $request_filename)
    {
      rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
    }

    location ~ .(jsp|jspx|do)?$
    {
      proxy_set_header Host $host;
      proxy_set_header x-forwarded-for $remote_addr;
      proxy_pass http://tomcat_server;
    }

    location ~ .*.(php|php5)?$
    {
      try_files $uri =404;
      fastcgi_pass unix:/tmp/php-cgi.sock;
      fastcgi_index index.php;
      include fcgi.conf;
    }

    location /status {
      stub_status on;
      access_log off;
    }

    location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
    {
      expires 30d;
    }

    location ~ .*.(js|css)?$
    {
      expires 12h;
    }

    access_log /home/wwwlogs/access.log access;
  }
  include vhost/*.conf;
}
```

检查nginx.conf的配置文件语法  
`/usr/local/nginx/sbin/nginx -t`
启动nginx  
`/usr/local/nginx/sbin/nginx`

测试jsp
浏览器打开http://ip:8080
成功显示tomcat页面

至此，lnmp配置php与jsp共存成功
