# **运行Tomcat时出现的错误：已经为元素 "web-app" 指定属性 "xmlns"**


打开web.xml;最上面的web-app里面，

看有没有重复的，
重点关注xmlns="http://java.sun.com/xml/ns/javaee" ，如果重复，删去就好~~~