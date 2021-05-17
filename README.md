# nginx-study

### 1 windows nginx 配置

```nginx

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid  logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

   # access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

	 server {
        listen      80;
        server_name dev.xgjzhptte.cn;

        charset utf-8;

       # access_log  logs/xgjzhptte.access.log  main;

        location / {
            root   D:\\dist;
            index  index.html index.htm;
        }
   
        location /api/ {
		    proxy_pass  http://127.0.0.1:8899/;
            proxy_set_header  Host  $host;
            proxy_set_header  X-Real-IP  $remote_addr;
            proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_connect_timeout 1800;
            proxy_send_timeout 1800;
            proxy_read_timeout 1800;
            proxy_buffer_size 4k;
            proxy_buffers 4 32k;
            proxy_busy_buffers_size 64k;
            proxy_temp_file_write_size 64k;
            client_max_body_size 100m;
            client_body_buffer_size 128k;
        }
	}
	
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}


```

前端打包的baseUrl为  http://dev.xgjzhptte.cn/api

后端的接口地址为： http://127.0.0.1:8899/

### 2 启动、停止、重启命令

```ngnix

启动：cmd 到根目录
C:\server\nginx-1.0.2>start nginx

 停止：
 C:\server\nginx-1.0.2>nginx.exe -s stop
 C:\server\nginx-1.0.2>nginx.exe -s quit
 
注：stop是快速停止nginx，可能并不保存相关信息；quit是完整有序的停止nginx，并保存相关信息

重新载入nginx
C:\server\nginx-1.0.2>nginx.exe -s reload
当配置信息修改，需要重新载入这些配置时使用此命令。

重新打开日志文件
C:\server\nginx-1.0.2>nginx.exe -s reopen

查看nginx版本
C:\server\nginx-1.0.2>nginx -v


```



### 配置2

```nginx


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

   # access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

	
	 server {
        listen      80;
        server_name xgjzhptte.cn;

        charset utf-8;

       # access_log  logs/xgjzhptte.access.log  main;

        location / {
            root   D:\\whl-dist;
            index  index.html index.htm;
        }
   
        location /192.168.1.184:8888 {
		    proxy_pass  http://192.168.1.184:8888/;
            proxy_set_header  Host  $host;
            proxy_set_header  X-Real-IP  $remote_addr;
            proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_connect_timeout 1800;
            proxy_send_timeout 1800;
            proxy_read_timeout 1800;
            proxy_buffer_size 4k;
            proxy_buffers 4 32k;
            proxy_busy_buffers_size 64k;
            proxy_temp_file_write_size 64k;
            client_max_body_size 100m;
            client_body_buffer_size 128k;
        }
	}
	
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}



```

后台接口 192.168.1.184:8888











