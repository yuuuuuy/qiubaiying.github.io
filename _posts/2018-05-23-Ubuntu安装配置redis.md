---
layout:     post
title:      Ubuntu安装配置redis
subtitle:   Ubuntu使用包管理工具安装redis
date:       2018-05-23
author:     Jae
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - Linux
    - redis
---

### 安装Redis
    sudo apt-get install redis-server

#### 查看redis运行状态

    ps -aux | grep redis
    sudo /etc/init.d/redis-server status

### 修改相应配置

##### 设置redis服务器密码,默认是不需要密码的
    sudo vim /etc/redis/redis.conf
    可以添加配置
    requirepass [Your Password]

###### 允许redis服务器被远程访问
    默认情况下只是允许本机访问，需要修改如下配置
    sudo vim /etc/redis/redis.conf
    bind 127.0.0.1  修改为 bind 0.0.0.0

##### 重启redis
    sudo /etc/init.d/redis-server restart

##### END
