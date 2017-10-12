# nginx


## [安装](http://www.runoob.com/linux/nginx-install-setup.html)

建立目录: mkdir /nginx

进入目录下载安装nginx时指定配置文件: ./configure --prefix=/nginx

安装完毕,查看nginx版本: 进入/nginx/sbin目录,执行 ./nginx -v 即可查看版本号.

运行nging:  ./nginx


开启80端口: firewall-cmd --zone=public --add-port=80/tcp --permanent

即可访问.


## [学习篇](http://tengine.taobao.org/book/chapter_02.html)

./nginx -s reload #重启nginx
./nginx -s stop   #就是来停止nginx的运行

### nginx的进程模型
![](http://tengine.taobao.org/book/_images/chapter-2-1.PNG)

现在，我们知道了当我们在操作nginx的时候，**nginx内部做了些什么事情**，那么，**worker进程又是如何处理请求的呢？**

我们前面有提到，worker进程之间是平等的，每个进程，处理请求的机会也是一样的。**当我们提供80端口的http服务时，一个连接请求过来，每个进程都有可能处理这个连接，怎么做到的呢？**

首先，每个worker进程都是从master进程fork过来，在master进程里面，先建立好需要listen的socket（listenfd）之后，然后再fork出多个worker进程。所有worker进程的listenfd会在新连接到来时变得可读，为保证只有一个进程处理该连接，所有worker进程在注册listenfd读事件前抢accept_mutex，抢到互斥锁的那个进程注册listenfd读事件，在读事件里调用accept接受该连接。当一个worker进程在accept这个连接之后，就开始读取请求，解析请求，处理请求，产生数据后，再返回给客户端，最后才断开连接，这样一个完整的请求就是这样的了。我们可以看到，一个请求，完全由worker进程来处理，而且只在一个worker进程中处理。

### 异步非阻塞的方式来处理请求


**nginx采用多worker的方式来处理请求，每个worker里面只有一个主线程，那能够处理的并发数很有限啊，多少个worker就能处理多少个并发，何来高并发呢？**

这就是nginx的高明之处，nginx采用了异步非阻塞的方式来处理请求

**为什么nginx可以采用异步非阻塞的方式来处理呢，或者异步非阻塞到底是怎么回事呢？**




## nginx配置文件详解

下面开始配置nginx,及反向代理，编辑配置文件nginx.conf
    vim /usr/local/nginx/conf/nginx.conf


   user nginx nginx;                                   #这里是nginx运行的用户

   worker_processes 2;                            #设置nginx服务的worker子进程，比如设为2：

   error_log logs/error.log;                        #去掉前面的#，记录nginx错误日志，方便检查bug：

   pid logs/nginx.pid;                                 #nginx的pid位置


events {
             worker_connections  1024;       #每个进程允许的最多连接数，
 }

http {

      include   mime.types;

     default_type  application/octet-stream;

   #把下面的#去掉，这是日志配置：

   log_format  main  '$remote_addr - $remote_user [$time_local] "$request"     '

                       '$status $body_bytes_sent "$http_referer" '

                       '"$http_user_agent" "$http_x_forwarded_for"';

   access_log logs/access.log main;                      #日志存放位置


#这里很关键，很多小伙伴问我 “负载均衡乍配置，为啥我配置的不能访问呢“，这里的upstream就是配置负载均衡的，当然得两台以上才叫负载，我这里的ip69和68都是

#用的apache,   也许你们的是tomcat, 没关系，按这样配置一样可以，

 upstream proxy_test {

   server 192.168.4.69:80 weight=1;     #如果你要测试，把这里换成你自己要代理后端的ip

   server 192.168.4.68:80 weight=1;

   #ip_hash;                                              #当负载两台以上用ip来hash解决session的问题，一台就别hash了。

 }

这是server段的配置

server {

    listen       80;

    server_name  www.test.com;    #要访问的域名，我这里用的测试域名，如果有多个，用逗号分开


    charset utf8;


    location / {

        proxy_pass       http://proxy_test;               #这里proxy_test是上面的负载的名称，映射到代理服务器，可以是ip加端口,   或url 

        proxy_set_header Host      $host;

        proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

      }

   }

}
保存退出！

nginx平滑重启：nginx -s reload   #加载刚刚加入的配置。









