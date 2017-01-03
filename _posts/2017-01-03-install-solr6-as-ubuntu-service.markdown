---
layout: post
title: install solr6 as ubuntu service
date: 2016-12-15 15:00:00.000000000 +08:00
------------------------------------------

# 环境
* Ubuntu 12.04 
* Solr 6.3.0

1. 到官网下载 solr-6.3.0.tgz

2. 解压solr-6.3.0.tgz , 得到install_solr_service.sh
```shell
 tar xzf solr-6.3.0.tgz solr-6.3.0/bin/install_solr_service.sh --strip-components=2
```

3. 安装solr
```shell
 sudo bash ./install_solr_service.sh solr-X.Y.Z.tgz -i /opt -d /var/solr -u solr -s solr -p 8983
```


##### 参考文档
* [Taking Solr to Production](https://cwiki.apache.org/confluence/display/solr/Taking+Solr+to+Production)
