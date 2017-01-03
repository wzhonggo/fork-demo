---
layout: post
title: install solr6 as ubuntu service
date: 2017-01-03 15:00:00.000000000 +08:00
------------------------------------------

# 环境
* Ubuntu 12.04 
* Solr 6.3.0
* JDK8

1. 到官网下载 solr-6.3.0.tgz (JDK版本必须大于等于8)

2. 解压solr-6.3.0.tgz , 得到install_solr_service.sh
```shell
 tar xzf solr-6.3.0.tgz solr-6.3.0/bin/install_solr_service.sh --strip-components=2
```

3. 安装solr
```shell
 sudo bash ./install_solr_service.sh solr-X.Y.Z.tgz -i /opt -d /var/solr -u solr -s solr -p 8983
```


4. 在 /etc/default/solr.in.sh 里面可以到安装完成后的生成的属性，里面可以手动设置时区，内存大小等
```
SOLR_JAVA_HOME="/opt/jdk1.8.0_66/"
#内存
SOLR_HEAP="3000m"
# By default the start script uses UTC; override the timezone if needed
# 时区
SOLR_TIMEZONE="UTC+8"
```

##### 参考文档
* [Taking Solr to Production](https://cwiki.apache.org/confluence/display/solr/Taking+Solr+to+Production)
