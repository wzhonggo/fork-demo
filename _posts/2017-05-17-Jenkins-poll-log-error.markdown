---
layout: post
title: Jenkins git polling log error with code 128
date: 2017-05-17 18:00:00.000000000 +08:00
---

# 环境
* Jenkins 1.x ,2.x
* git 2.11

##### git远程仓库有新的commit ,但是jenkins不能触发自动编译， 但是手动点击编译是可以的
* git 仓库配置是https协议的，用户名和密码直接写带url上面
* jenkins git Poll SCM  配置
  ```txt
  H H/1 * * *
  ```
* Git Polling Log
   ```
   git.exe -c core.askpass=true ls-remote -h https://useranme@bitbucket.org/test/demo.git 128 错误
   ```

##### 解决办法
  * 如果git仓库是https协议，通过设置下面的git命令，让git记住用户名和密码
  ``` git
  git config --global credential.helper store
    ```
  * 或者使用ssh协议，这样就不需要密码(推荐)


##### 参考文档
* [Git 工具 - 凭证存储](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%87%AD%E8%AF%81%E5%AD%98%E5%82%A8)