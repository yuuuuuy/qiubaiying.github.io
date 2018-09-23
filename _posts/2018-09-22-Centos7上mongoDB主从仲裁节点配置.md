---
layout:     post
title:      Centos7上MongoDB主从仲裁节点配置
subtitle:   在Centos上MongoDB主从仲裁节点的配置
date:       2018-09-22
author:     Jae
header-img: img/post-bg-debug.png
catalog: true
tags:
    - mongodb
    - Linux
---
### 写在前面
熟悉项目需要部署相应的集群，这篇文章只是为了记录自己在搭建mongodb主从和仲裁节点的过程。

### 软件版本
     Centos7三台，内网IP分别为：
     192.168.109.129 --> mongo1
     192.168.109.130 --> mongo2
     192.168.109.131 --> mongo3

     mongodb-2.6.12 版本过低


### 安装mongodb
三台机器相同的安装方法

1、添加仓库

     在/etc/yum.repos.d/目录下创建文件mongodb.repo，
     它包含MongoDB仓库的配置信息，内容如下：
     复制代码代码如下:
      [mongodb]
      name=MongoDB Repository
      baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64/
      gpgcheck=0
      enabled=1

2、执行安装命令

     $ sudo yum install mongodb-org
     启动mongod
     sudo systemctl start mongod
     加入开机启动项
     sudo /sbin/chkconfig mongod on

3、在mongo1(主)中创建一个单独的数据库和用户

     use admin
     db.createUser(
         {
             user: "admin",
             pwd: "admin",
             roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
         }
         )
     db.auth("admin","admin") //验证是否添加用户
     //切换到我们的数据库
     use mydata
     db.createUser(
         {
             user: "test",
             pwd: "test",
             roles: [ { role: "readWrite", db: "mydata" } ]
         )
     db.auth("test","test")

4、三台机器都执行以下操作

     echo 'replSet = testre' >> /etc/mongod.conf
     编辑/etc/mongod.conf文件，注释掉bind_ip=127.0.0.1

5、修改主机名

例如执行命令```sudo hostnamectl set-hostname  mongo1```将第一台主机名修改为mongo1，三台机器执行类似的操作。

6、分别修改三台机器的hosts文件

    对于mongo1
    sudo vim /etc/hosts
    127.0.0.1              mongo1
    192.168.109.130        mongo2
    192.168.109.131        mongo3

    对于mongo2
    sudo vim /etc/hosts
    127.0.0.1              mongo2
    192.168.109.129        mongo1
    192.168.109.131        mongo3

    对于mongo3
    sudo vim /etc/hosts
    127.0.0.1              mongo3
    192.168.109.129        mongo1
    192.168.109.130        mongo2

7、三台机器都使用iptables开放27107端口
Centos7默认使用firewall作为防火墙，当使用iptables需要重新配置

     systemctl stop firewalld #停止firewall
     systemctl disable firewalld #禁止firewall开机启动
     //安装iptables-services
     yum -y install iptables-services
     //添加策略
     iptables -I INPUT -p tcp --dport 27017 -j DROP
     iptables -I INPUT -p tcp -s 192.168.109.0/24 --dport 27017 -j ACCEPT  //允许该网段的所有主机访问
     iptables -I INPUT -p tcp -s 127.0.0.1 --dport 27017 -j ACCEPT
     //保存
     service iptables save
     //开机自启
     systemctl enable iptables

8、设置主从仲裁

     在mongo1中进入mongo命令行
      > rs.initiate()
      //mongo1，增加second节点
      > rs.add("mongo2:27017")
      //mongo1增加仲裁节点
      testre:PRIMARY> rs.addArb("mongo3:27017")
      //插卡nreplset状态
      testre:PRIMARY> rs.status()

三台机器中智能有一台运行```rs.initiate()```,如果其他的机器运行了删除重新安装。

     systemctl stop mongod
     yum erase $(rpm -qa | grep mongodb-org)
     rm -rf  /var/log/mongodb
     rm -rf /var/lib/mongo

9、查看配置结果
到这里基本配置好了主从和仲裁节点
我们可以进入mongo1中


    testre:PRIMARY> rs.status()
    {
       "set" : "mydata",
       "date" : ISODate("2018-09-23T02:41:13Z"),
       "myState" : 1,
       "members" : [
               {
                       "_id" : 0,
                       "name" : "mongo1:27017",
                       "health" : 1,
                       "state" : 1,
                       "stateStr" : "PRIMARY",
                       "uptime" : 1085,
                       "optime" : Timestamp(1537624382, 1),
                       "optimeDate" : ISODate("2018-09-22T13:53:02Z"),
                       "electionTime" : Timestamp(1537670457, 1),
                       "electionDate" : ISODate("2018-09-23T02:40:57Z"),
                       "self" : true
               },
               {
                       "_id" : 1,
                       "name" : "mongo2:27017",
                       "health" : 0,
                       "state" : 8,
                       "stateStr" : "SECONDARY",
                       "uptime" : 0,
                       "optime" : Timestamp(1537624382, 1),
                       "optimeDate" : ISODate("2018-09-22T13:53:02Z"),
                       "lastHeartbeat" : ISODate("2018-09-23T02:40:55Z"),
                       "lastHeartbeatRecv" : ISODate("1970-01-01T00:00:00Z"),
                       "pingMs" : 0
               },
               {
                       "_id" : 2,
                       "name" : "mongo3:27017",
                       "health" : 1,
                       "state" : 7,
                       "stateStr" : "ARBITER",  //仲裁
                       "uptime" : 1085,
                       "lastHeartbeat" : ISODate("2018-09-23T02:41:12Z"),
                       "lastHeartbeatRecv" : ISODate("2018-09-23T02:41:12Z"),
                       "pingMs" : 1
               }
       ],
       "ok" : 1
    }
    testre:PRIMARY>

从上面的信息可以看到中从仲裁节点均正常运行；
现在我们将主节点的mongo服务关闭再验证mongo2会不会被选为主节点。
我们进入mongo2的mongo命令行：

    testre:PRIMARY> rs.status()
    {
       "set" : "mydata",
       "date" : ISODate("2018-09-23T02:45:52Z"),
       "myState" : 1,
       "members" : [
               {
                       "_id" : 0,
                       "name" : "mongo1:27017",
                       "health" : 0,
                       "state" : 8,
                       "stateStr" : "(not reachable/healthy)",
                       "uptime" : 0,
                       "optime" : Timestamp(1537624382, 1),
                       "optimeDate" : ISODate("2018-09-22T13:53:02Z"),
                       "lastHeartbeat" : ISODate("2018-09-23T02:45:52Z"),
                       "lastHeartbeatRecv" : ISODate("2018-09-23T02:45:15Z"),
                       "pingMs" : 0
               },
               {
                       "_id" : 1,
                       "name" : "mongo2:27017",
                       "health" : 1,
                       "state" : 1,
                       "stateStr" : "PRIMARY",  //主
                       "uptime" : 176,
                       "optime" : Timestamp(1537624382, 1),
                       "optimeDate" : ISODate("2018-09-22T13:53:02Z"),
                       "electionTime" : Timestamp(1537670719, 1),
                       "electionDate" : ISODate("2018-09-23T02:45:19Z"),
                       "self" : true
               },
               {
                       "_id" : 2,
                       "name" : "mongo3:27017",
                       "health" : 1,
                       "state" : 7,
                       "stateStr" : "ARBITER",
                       "uptime" : 169,
                       "lastHeartbeat" : ISODate("2018-09-23T02:45:51Z"),
                       "lastHeartbeatRecv" : ISODate("2018-09-23T02:45:51Z"),
                       "pingMs" : 1
               }
       ],
       "ok" : 1
    }
    testre:PRIMARY>

现在mongo2已经成为主节点。
