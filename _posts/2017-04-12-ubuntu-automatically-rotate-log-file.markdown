---
layout: post
title: ubuntu automatically rotate log file
date: 2017-04-12 16:00:00.000000000 +08:00
---

# 环境
* Ubuntu 14.04

# 查看apache2和tomcat7的自动rotate
- apache2 和tomcat7 是通过 apt-get install 安装
-  进入 rotate 的 配置目录 查看apache2 rotate 配置
```bash
cd /etc/logrotate.d/
vim apache2
 ```

# logrotate 部分参数
- monthly: 日志文件将按月轮循。其它可用值为‘daily’，‘weekly’或者‘yearly’。
- rotate 5: 一次将存储5个归档日志。对于第六个归档，时间最久的归档将被删除。
- compress: 在轮循任务完成后，已轮循的归档将使用gzip进行压缩。
- delaycompress: 总是与compress选项一起用，delaycompress选项指示logrotate不要将最近的归档压缩，压缩将在下一次轮循周期进行。这在你或任何软件仍然需要读取最新归档时很有用。
- missingok: 在日志轮循期间，任何错误将被忽略，例如“文件无法找到”之类的错误。
- notifempty: 如果日志文件为空，轮循不会进行。
- create 644 root root: 以指定的权限创建全新的日志文件，同时logrotate也会重命名原始日志文件。
- postrotate/endscript: 在所有其它指令完成后，postrotate和endscript里面指定的命令将被执行。在这种情况下，rsyslogd 进程将立即再次读取其配置并继续运行。

# 创建一个新的rotate配置
- 在/home/ubuntu新建一个文件test.log,向test.log里面写入内容
- 在/etc/logrotate.d/ 目录下面新建一个文件 test
- 复制下面内容到 test文件中
```bash
/home/ubuntu/test.log {
  copytruncate
  daily
  rotate 7
  compress
  missingok
  size 1M
}
```
- logrotate是用cron来执行的，可以查看 /etc/cron.daily/logrotate， 最终所有放在/etc/logrotate.d/目录的配置都会被读取到，就是触发rotate test.log


##### 参考文档
* [How to Rotate Tomcat catalina.out](https://dzone.com/articles/how-rotate-tomcat-catalinaout)
* [How To Manage Log Files With Logrotate On Ubuntu 12.10](https://www.digitalocean.com/community/tutorials/how-to-manage-log-files-with-logrotate-on-ubuntu-12-10)
* [Linux日志文件总管——logrotate](https://linux.cn/article-4126-1.html)
