---
layout: post
title: AWS ELB 504 with Apache2
date: 2016-12-15 18:00:00.000000000 +08:00
---

# 环境
* Ubuntu 14.04 
* Apache 2.4.7
* Tomcat7

1. mpm_worker 不会出现504 ，mpm_event 可能会出现504 ，出错概率在万分之一左右，出错的时候apache access log看不到出错的request url ， apache2 error log 里面也看不到这个 request url

2. 用 jmeter 测试的时候最大的response time 超过 ELB idle time 就会出现504