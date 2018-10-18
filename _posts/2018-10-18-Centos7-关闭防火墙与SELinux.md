---
layout:     post
title:      Centos7关闭防火墙与SeLinux
subtitle:   Centos7关闭防火墙与SeLinux
date:       2018-10-18
author:     Jae
header-img: img/post-bg-digital-native.jpg
catalog: true
tags:
    - Linux
---

##### 1、Centos7默认的管理防火墙规则的工具
Centos&默认使用的是firewalld来对防火墙规则进行管理。
##### 2、iptables和firewalld区别
**iptables**是**Linux**上管理防火墙规则的工具，但是**firewalld**也是**Linux**上管理防火墙规则的工具。其实除了前面这两种工具还有一种工具叫做
**netables**。

这一切都从**Netfilter**开始，他是在**Linux**内核模块级别控制访问网络栈，几十年来管理**Netfilter**钩子的主要命令工具是**iptables**规则集。因为调用这些规则的语法晦涩难懂，为了用户更友好的使用，**ufw**和**firewalld**被引入，作为更高级别的**Netfilter**解释器。

但是**ufw**和**firewalld**主要是为了解决单独的计算机所面临的各种问题所设计，所以构建全面的网络解决方案通常还是需要**iptables**。
##### 3、关闭firewalld

    systemctl stop firewalld.service  停止firewalld
    systemctl disable firewalld.service 禁止firewall开机自启

##### 4、使用iptables
    yum -y install iptables-services

通过编辑配置文件来增加策略，如开放8080端口

    vi /etc/sysconfig/iptables
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT

重启防火墙并设置开机自启

    systemctl restart iptables.service
    systemctl enable iptables.service
##### 5、什么是SeLinux
安全增强式Linux（[SELinux](https://zh.wikipedia.org/wiki/%E5%AE%89%E5%85%A8%E5%A2%9E%E5%BC%BA%E5%BC%8FLinux)，Security-Enhanced Linux）是一组给Linux核心的补丁，并提供一些更强、更安全的强制访问控制架构来和核心的主要子系统共同运作。
##### 6、关闭SELinux

    vi /etc/sysconfig/selinux
    修改SELINUX=enforcing为disabled
    SELINUX=disabled
    保存并执行
    setenforce 0

查看SELinux 状态

    [root@bogon ~]# getenforce
    Permissive 已经关闭

##### 7、NED
