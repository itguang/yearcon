#  linux异常系列Can't connect to X11 window server


今天项目上线测试,开发环境是windows7+JDK8+tomcat8

生产环境是 centos6.8+JDK8+tomcat8

在本机测试没问题,然后部署到服务器,发现一个奇怪的异常

```
 Can't connect to X11 window server using '0.0' as the value of the DISPLAY variable.
```

而且这个异常出现的时机非常有意思,就是当我的程序用到画图,比如生成Excel或者验证码图片时,就会报此异常信息,

后来发现原因,原来之前管理服务器的哥们在上面装了个图形界面,真是坑,你说一个服务其装什么图形界面啊!!!

那么既然找到原因: 因为用到了图形处理，java程序会去寻找linux上的图形界面是否启动

解决方法自然就有了:

```
解决：
在.bash_profile文件最后 加了个export JAVA_OPTS=-Djava.awt.headless=true

注意linux发行版本可能文件名不一样 可能为 .bashrc
```
再次重启tomcat,问题解决!
















