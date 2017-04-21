---
layout: post
title: IntelliJ IDEA with JRebel
date: 2017-04-21 18:00:00.000000000 +08:00
---

# 环境
IntelliJ-IDEA 2017.1
JRabel IntelliJ-IDEA plugin 7.0.7
JDK8

# IntelliJ IDEA 中 使用JRebel plugin
- 到 [jreble](https://my.jrebel.com/) 注册账号， 根据提示得到License code
- 在 IntelliJ IDEA 安装 JRebel plugin， 用上面的license code激活
- 在 IntelliJ IDEA settings 里面找到 compiler , enable "Buld project automatically" 和 "Compile independent modules in parallel"
- 新建一个普通的maven java web工程 demo
- 在IntelliJ IDEA Run/Debug configurations 里面新建一个local tomcat
    - 在 Server 面板 On 'Update' action 选择 "Update classes and resources"
    - 在 Server 面板   On frame deactivation 选择"Update classes and resources"
    - 在 Deployment 面板 选择 demo:war exploded
    - 如果出现java.lang.OutOfMemoryError: PermGen space   , 在Runner面板 VM Options 添加 jvm参数 -server -XX:PermSize=256M -XX:MaxPermSize=512m -Dfile.encoding=UTF-8
- 在JRebel Panel enable JRebel(会在maven 工程的resources目录生成rebel.xml)
- 然后选择 JRebel debug启动就会看到下面日志，表示JRebel成功
```txt
2017-04-21 18:13:44 JRebel: Contacting myJRebel server ..
2017-04-21 18:13:47 JRebel:  Starting logging to file: C:\Users\DJH\.jrebel\jrebel.log
2017-04-21 18:13:47 JRebel:
2017-04-21 18:13:47 JRebel:  #############################################################
2017-04-21 18:13:47 JRebel:
2017-04-21 18:13:47 JRebel:  JRebel Agent 7.0.7 (201704100855)
2017-04-21 18:13:47 JRebel:  (c) Copyright ZeroTurnaround AS, Estonia, Tartu.
2017-04-21 18:13:47 JRebel:
2017-04-21 18:13:47 JRebel:  Over the last 2 days JRebel prevented
2017-04-21 18:13:47 JRebel:  at least 17 redeploys/restarts saving you about 0.7 hours.
2017-04-21 18:13:47 JRebel:
2017-04-21 18:13:47 JRebel:  Licensed to wzhonggo go (using myJRebel).
2017-04-21 18:13:47 JRebel:
2017-04-21 18:13:47 JRebel:
2017-04-21 18:13:47 JRebel:  #############################################################
2017-04-21 18:13:47 JRebel:
```
- 随便修改一个java 文件， 会到console 日志里面看到 下面日志
```txt
2017-04-21 18:15:31 JRebel: Reloading class 'com.demo.UserController'.
2017-04-21 18:15:32 JRebel: Reinitialized class 'com.demo.UserController$$EnhancerBySpringCGLIB$$45800121'.
```

- 如果使用的是 tomcat7-maven-plugin 或者是jetty-maven-plugin 来启动 web项目， 在修改java文件后，需要手动Recompile修改过的文件(Ctrl+Shift+F9),
然后也会看到上面JRebel reloading修改过的文件的日志，
    - 如果使用jetty-maven-plugin， 手动Recompile 导致整个应用重启， 需要关掉自动热部署
```xml
  <!-- 自动热部署 -->
 <reload>manual</reload>
```


##### 参考文档
[IntelliJ IDEA 的 Java 热部署插件 JRebel 安装及使用](https://github.com/judasn/IntelliJ-IDEA-Tutorial/blob/newMaster/jrebel-setup.md)
[IDEA 如何使用JRebel 部署web项目呢?](https://yq.aliyun.com/articles/41697)
[Application configuration using rebel.xml](https://manuals.zeroturnaround.com/jrebel/standalone/config.html#rebel-xml)