# linux


## linux切换用户

ubuntu如何切换到root用户

默认安装完成之后并不知道root用户的密码，那么如何应用root权限呢？

(1)sudo 命令  
```
xzm@ubuntu:~$  sudo
```
这样输入当前管理员用户密码就可以得到超级用户的权限。但默认的情况下5分钟root权限就失效了。

(2)sudo -i
```
xzm@ubuntu:~$  sudo -i
```
通过这种方法输入当前管理员用户的密码就可以进到root用户。

(3)如果想一直使用root权限，要通过su切换到root用户。

那我们首先要重设置root用户的密码：
```
xzm@ubuntu:~$  sudo passwd root
```
这样就可以设置root用户的密码了。

之后就可以自由的切换到root用户了
```
xzm@ubuntu:~$  su
```
输入root用户的密码即可。

## 在当前目录下查找某文件

例如:
```
find -name tomcat
```


## 用rpm命令来查看是否安装了FTP服务。

如果您的linux系统是centos，可以用命令rpm -qa | grep ftp来查看是否安装了FTP服务。

例如:
```
rpm -qa | grep ftp
```

# linux查看进程

例如:
```
ps aux|grep redis
```
结果:
```
root     21677  0.0  0.0 143912  7684 ?        Ssl  10:06   0:00 ./src/redis-server 127.0.0.1:6379
root     21848  0.0  0.0 103316   864 pts/0    S+   10:14   0:00 grep redis
```

杀死进程
```
kill -9 21677
```


# 防火墙重启(service iptables restart)

修改/etc/sysconfig/iptables 文件，增加如下一行：
```
　　-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```

永久性生效，重启后不会复原

开启： chkconfig iptables on

关闭： chkconfig iptables off

2） 即时生效，重启后复原

开启： service iptables start

关闭： service iptables stop

查询TCP连接情况：

 netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'

查询端口占用情况：

 netstat   -anp   |   grep  portno（例如：netstat –apn | grep 80）

# 粘贴复制

  yy    复制整行（nyy或者yny ，复制n行，n为数字）

  p      小写p代表贴至游标后（下），因为游标是在具体字符的位置上，所以实际是在该字符的后面 

在vi中按u可以撤销一次操作

u   撤销上一步的操作
Ctrl+r 恢复上一步被撤销的操作


首先，在命令行模式输入ndd，将剪切当前行 + 随后的n-1行
然后，将光标移动到需要的位置，然后按p
 
删除文本内容：  dG

# 环境变量
http://www.cnblogs.com/xdp-gacl/p/4097608.html
```
cd /etc/profile
```







# [centos7 mysql 安装](http://www.cnblogs.com/hwd-cnblogs/p/5213337.html)

 1.启动命令

systemctl start mysqld.service 







