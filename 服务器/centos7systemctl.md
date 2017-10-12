# [Centos 7 firewall 命令：](http://blog.csdn.net/jack85986370/article/details/51169203)


# [centos7下使用yum安装mysql数据库以及设置远程访问](http://www.cnblogs.com/jerrylz/p/5645224.html)

## 登陆mysql命令:

$ mysql -u root

重置密码：

$ mysql -u root
mysql > use mysql;
mysql > update user set password=password('123456') where user='root';
mysql > exit;

## 设置远程登陆

 创建普通用户并授权

示例（使用root用户登录）：

mysql > use mysql;
mysql > grant all privileges on *.* to 'root'@'%' identified by '123456';
mysql > flushn privileges;


# 开放端口

开启端口

firewall-cmd --zone=public --add-port=80/tcp --permanent

命令含义：

--zone #作用域

--add-port=80/tcp  #添加端口，格式为：端口/通讯协议

--permanent  #永久生效，没有此参数重启后失效

重启防火墙

firewall-cmd --reload

直接关闭防火墙

systemctl stop firewalld.service #停止firewall

systemctl disable firewalld.service #禁止firewall开机启动

开启防火墙：

systemctl stop firewalld.service  
systemctl start firewalld.service  


查看状态
# firewall-cmd --state //running 表示运行