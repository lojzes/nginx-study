### 1 反向代理与正向代理

> 正向代理：通过代理服务器访问网络。
>
> 反向代理：通过反向代理服务器，把请求转发到目标服务器，然后返回数据。暴露的是代理服务器的ip,隐藏了真实服务器ip。

### 2 负载均衡

### 3 动静分离 

### 4 安装

docker 常用命令

```shell
docker stop $(docker ps -aq)   // 停止索引容器
docker rm $(docker ps -aq)     // 删除所有容器
```

docker-compose.yml:

```yaml
version: "3.7"
services:
  web:
    image: nginx:1.12.2
    container_name: nginx1.12
    volumes:
     - /docker-soft/ngnix/conf/nginx.conf:/etc/nginx/nginx.conf:ro
     - /docker-soft/ngnix/logs/nginx:/var/log/nginx
    restart: always
    ports:
     - "80:80"
     - "9001:9001"
    environment:
     - NGINX_HOST=foobar.com
    networks:
        nginx-network:
          ipv4_address: 172.19.0.2   

  tomcat1:
    image: tomcat:8.5
    container_name: tomcat8.5_1
    restart: always
    ports:
       - "8080:8080"
    volumes:
      - /docker-soft/ngnix/tomcat/apps8080:/usr/local/tomcat/webapps
      - /docker-soft/ngnix/tomcat/tomcat_conf/t_1:/usr/local/tomcat/conf
    networks:
        nginx-network:
          ipv4_address: 172.19.0.5   

  tomcat2:
    image: tomcat:8.5
    container_name: tomcat8.5_2
    restart: always
    ports:
      - "8081:8080"
    volumes:
      - /docker-soft/ngnix/tomcat/apps8081:/usr/local/tomcat/webapps  
      - /docker-soft/ngnix/tomcat/tomcat_conf/t_2:/usr/local/tomcat/conf
    networks:
        nginx-network:
          ipv4_address: 172.19.0.6     

networks:
   nginx-network:
      ipam:
         config:
           - subnet: 172.19.0.0/16   
    
   
```

### 5 常用命令

1 查看版本

```shell
ngnix -v
```

2 关闭

```shell
ngnix -s stop
```

3 启动

```shell
./nginx 
```

4 重新加载

```shell
ngnix -s reload
```

### 6配置文件

一共分3块

- 全局块

  > ngnix 服务整体运行的配置
  >
  > 如 worker_processes

- events

  > ngnix 与用户连接的配置
  >
  > 如 worker_conntections. 1024

- http

  > 又包括http块和server 

  

### 7 例子：

输入http://192.168.1.11:80 时会把请求转发到http://192.168.1.11:8080

```shell
server {
        listen       80;
        server_name  192.168.1.11;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            proxy_pass  http://192.168.1.11:8080;
            index  index.html index.htm;
        }

```

### 8 例子

http://192.168.1.11:9001/edu/a.html   转发到 172.19.0.5:8080

http://192.168.1.11:9001/vod/a.html  转发到 172.19.0.6:8081

```shell
 server {
       listen               9001;
       server_name          192.168.1.11;

       location ~ /edu/  {
             proxy_pass  http://172.19.0.5:8080;
         }

       location ~ /vod/  {
             proxy_pass   http://172.19.0.6:8081;
         }

    }
```

***注意：docker 方式运行tomcat,在指定转发地址时，要写上容器内的ip,不能写127.0.0.1或者本机ip***














