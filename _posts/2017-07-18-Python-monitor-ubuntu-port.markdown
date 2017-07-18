---
layout: post
title: python monitor ubuntu port
date: 2017-07-18 18:00:00.000000000 +08:00
---
# 环境
* Ubuntu 14.04
* Python 2.7
* JDK8

# 通过python监控ubuntu上面的端口
* 通过crontab调用shell脚本， shell脚本里面会根据指定的端口去查找是否能连接上，如果不能连接会先发送消息，然后尝试重启改端口对应的服务，
如果重启成功会发送重启成功的消失，失败会发送失败的消失，通过修改javaCmd,让具体消失发送可以是email，也可以是其他的。

# monitor.sh
``` bash
#!/bin/bash
cd /home/ubuntu/SNS/
#/home/ubuntu/SNS/ is monitor.py , monitor.sh 所在的目录
python monitor.py -p 80 -p 3306 -p 8080 -s localhost
```

# monitor.py
* [monitor.py 源码](https://gist.github.com/wzhonggo/c1fa0dda6510535441b96102d5c0b26c)
* ubuntu上面java -classpath 命令参数用 **:** 隔开而不是windows上面的 **;**

# 添加到corntab
* 在可以sudo 的用户的crontab 里面加上下面命令, 每过十分钟就会执行一次monitor.sh
``` bash
*/10 * * * *       /home/ubuntu/SNS/monitor.sh > /dev/null 2>&1
```

##### 参考文档