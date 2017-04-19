---
layout: post
title: eclipse information center
date: 2017-04-19 18:00:00.000000000 +08:00
---

# 环境
Eclipse Neon
JDK8

# 搭建 eclipse help server
- 下载Eclipse Neon
- 安装Neon 在 D:\test 目录， 会创建 D:\test\eclipse目录,
- 在D:\test\eclipse\plugins 目录查看 eclipse help base jar 的版本，然后替换下面version, 在cmd窗口里面运行下面命令
```cmd
     java -classpath D:\test\eclipse\plugins\org.eclipse.help.base_[version].jar org.eclipse.help.standalone.Infocenter -command start -eclipsehome D:\test\eclipse -port 8081 -vmargs -Xms64M -Xmx128M
```

- 在浏览器访问 http://localhost:8081/help/index.jsp
- 关闭 help server 执行下面命令
```cmd
java -classpath D:\test\eclipse\plugins\org.eclipse.help.base_[version].jar org.eclipse.help.standalone.Infocenter -command shutdown -eclipsehome D:\test\eclipse
```


##### 参考文档
* [Eclipse Information Center](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Fguide%2Fua_help_setup_infocenter.htm)