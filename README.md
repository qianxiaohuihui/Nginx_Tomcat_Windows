# Nginx_Tomcat_Windows
Windows搭建Nginx+Tomcat做负载均衡<br/>
Nginx监听端口为88<br/>
Tomcat的端口分别修改为8081和8082，版本为apache-tomcat-8.5.31

nginx-1.14.2\conf\nginx.conf中的配置如下：<br/><br/>

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    
	#服务器的集群  
    upstream  netitcast.com {                       #服务器集群名字   
        server    127.0.0.1:8082  weight=1;		#服务器配置   weight是权重的意思，权重越大，分配的概率越大。  
        server    127.0.0.1:8081  weight=1;  
    }   
    server {
        listen       88;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_pass http://netitcast.com;  
            proxy_redirect default;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        } 
    }
}



使用命令操作Nginx，先将路径跳转到Nginx所在目录。

启动nginx:		<br/>
nginx.exe	  	<br/>
start nginx		<br/>

停止nginx:		<br/>
nginx -s stop		<br/>
nginx -s quit		<br/>

查看nginx版本：		<br/>
nginx -v		<br/>
nginx -V		<br/>

重载nginx：		<br/>
nginx -s reload		<br/>

重新打开日志文件:	<br/>
nginx -s reopen
