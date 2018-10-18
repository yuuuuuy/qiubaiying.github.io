---
layout:     post
title:      Centos7安装nginx
subtitle:   Centos7安装nginx
date:       2018-10-18
author:     Jae
header-img: img/home-bg.jpg
catalog: true
tags:
    - Linux
---

##### 安装

    sudo vi /etc/yum.repos.d/nginx.repo

    # Enter the following to the file
    [nginx]
    name=nginx repo
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
    gpgcheck=0
    enabled=1

    sudo yum makecache
    sudo yum install nginx

#### 启动

    systemctl start nginx
