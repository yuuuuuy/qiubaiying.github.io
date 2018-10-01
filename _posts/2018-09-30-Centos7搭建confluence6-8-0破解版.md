---
layout:     post
title:      Centos7搭建confluence6.8.0破解版
subtitle:   confluence一款企业知识管理与协同软件
date:       2018-09-30
author:     Jae
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - Linux
---
### 写在前面
confluence是一个专业的企业知识管理和协作软件，可用于构建企业wiki，实现团队成员之间的协作。
confluence提供付费的服务，但是我们这次以confluence6.8.0版本在Centos7机器上搭建。
### 1、环境准备
##### 1.1 为Centos7换源

参考[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/centos/)的配置

##### 1.2 Java坏境配置

这里我们使用的jdk版本为1.8，具体的java环境配置网上资源比较多，就不再累述。


    [root@bogon ~]# java -version
    java version "1.8.0_181"
    Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
    Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)

### 2、下载confluence以及破解工具
注意我们准备的所有安装包都是先下载到本地，然后通过scp命令上传到服务器上。
##### 2.1 下载confluence对应的版本
[下载地址](https://www.atlassian.com/software/confluence/download-archives),这里我们下载6.8.0版本，最新的版本为测试版。在官网上下载文件大小为500+M，速度可能会比较慢。  
##### 2.2 下载破解包
链接：https://pan.baidu.com/s/1LDEwDZIyL2EJQJVEfMYlvg 密码：z3n0
我们下载下来后解压可以看到这样的内容
![解压后](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img1.png)
我的windows安装了java环境，直接双击```confluence_keygen.jar```就可以打开。

![双击.jar文件后](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img2.png)

我们破解分为两步，首先是将安装后的confluence库中的一个jar包替换掉，替换jar包使用的是上图箭头所指的按钮来实现，下面具体操作会有介绍。

### 3、安装confluence
###### 在下面需要执行linux命令时，如果没有给出```$```默认都是```root```下执行。
如下是我所有的安装包和依赖：

    [root@bogon data]# ls
    //confluence安装包
    atlassian-confluence-6.8.0-x64.bin
    //汉化
    Confluence-6.8.0-m56-language-pack-zh_CN.jar
##### 3.1 安装

    chmod +x atlassian-confluence-6.8.0-x64.bin
    ./atlassian-confluence-6.8.0-x64.bin

![开始安装](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img3.png)

在安装过程中，需要我们手动输入指令进行选择，前三次的输入如上图箭头所示，等待一会安装结束会询问是否现在启动服务，这里可以选择现在启动也可以不启动，因为一会破解要重启，所以这里选择不启动也可以。

    [root@bogon data]# ./atlassian-confluence-6.8.0-x64.bin
    Unpacking JRE ...
    Starting Installer ...
    Oct 01, 2018 3:17:13 AM java.util.prefs.FileSystemPreferences$1 run
    INFO: Created user preferences directory.
    Oct 01, 2018 3:17:13 AM java.util.prefs.FileSystemPreferences$2 run
    INFO: Created system preferences directory in java.home.

    This will install Confluence 6.8.0 on your computer.
    OK [o, Enter], Cancel [c]
    o
    Choose the appropriate installation or upgrade option.
    Please choose one of the following:
    Express Install (uses default settings) [1],
    Custom Install (recommended for advanced users) [2, Enter],
    Upgrade an existing Confluence installation [3]
    1
    See where Confluence will be installed and the settings that will be used.
    Installation Directory: /opt/atlassian/confluence
    Home Directory: /var/atlassian/application-data/confluence
    HTTP Port: 8090
    RMI Port: 8000
    Install as service: Yes
    Install [i, Enter], Exit [e]
    i

    Extracting files ...


    Please wait a few moments while we configure Confluence.
    Installation of Confluence 6.8.0 is complete
    Start Confluence now?
    Yes [y, Enter], No [n]
    n
    Installation of Confluence 6.8.0 is complete
    Finishing installation ...
    [root@bogon data]#

到这里已经安装完成，下面我们不着急破解，先把数据库安装好，如果想要在生产环境中搭建confluence，最好选择自己的数据库，因为他提供内置数据库用来演示或评估。
### 4、安装mysql
我们安装Mysql5.7版本，CentOS7系统开始，MariaDB成为yum 源中默认的数据库安装包。
##### 4.1 检查MariaDB是否安装

    yum list installed | grep mariadb
    mariadb-libs.x86_64                   1:5.5.56-2.el7                   @anaconda
##### 4.2 卸载全部 MariaDB 相关

    yum -y remove mariadb*

##### 4.3 下载MySQL的YUM源

    yum install wget
    wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

##### 4.4 安装YUM源

    rpm -ivh mysql57-community-release-el7-11.noarch.rpm

##### 4.5 查看是否安装完成

    [root@bogon data]# yum repolist enabled | grep "mysql.*-community.*"
    mysql-connectors-community/x86_64       MySQL Connectors Community           65
    mysql-tools-community/x86_64            MySQL Tools Community                69
    mysql57-community/x86_64                MySQL 5.7 Community Server          287
    [root@bogon data]#
说明已经安装成功

##### 4.6 安装Mysql

    yum install mysql-community-server

###### 安装完成后先不着急启动Mysql，我们先通过```/etc/my.cnf```文件对mysql进行配置
##### 4.7 设置编码为utf-8

修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示

    [mysqld]
    character_set_server=utf8
    init_connect='SET NAMES utf8'

##### 4.8 设置默认的事务隔离等级为READ-COMMIT

修改/etc/my.cnf配置文件，在[mysqld]下添加如下语句

    [mysqld]
    transaction-isolation=Read-Committed
##### 4.9 启动mysql

    systemctl start mysqld

##### 4.10 查看mysql状态

    [root@bogon data]# systemctl status mysqld
    ● mysqld.service - MySQL Server
    Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
    Active: active (running) since Mon 2018-10-01 03:37:43 EDT; 3s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
    Process: 23731 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
    Process: 23627 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
    Main PID: 23734 (mysqld)
    CGroup: /system.slice/mysqld.service
           └─23734 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

    Oct 01 03:37:38 bogon systemd[1]: Starting MySQL Server...
    Oct 01 03:37:43 bogon systemd[1]: Started MySQL Server.
    [root@bogon data]#

##### 4.11 设置开机自启

    systemctl enable mysqld
    systemctl daemon-reload

##### 4.12 修改root本地登录密码
mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码

    [root@bogon data]# grep 'temporary password' /var/log/mysqld.log
    2018-10-01T07:37:38.996309Z 1 [Note] A temporary password is generated for root@localhost: wSk0!mo2myp3
    密码为 wSk0!mo2myp3
首先给root设置我们自定义的密码
    mysql -u root -p
    mysql>ALTER USER 'root'@'localhost' IDENTIFIED BY 'Admin_123456';
注意Mysql对密码长度和负责度有要求，具体的可以网上搜索了解。
我们再来查看一下编码和全局事务隔离等级

    mysql>show variables like 'character_set_%';
    mysql> show variables like 'character_set_%';
    +--------------------------+----------------------------+
    | Variable_name            | Value                      |
    +--------------------------+----------------------------+
    | character_set_client     | utf8                       |
    | character_set_connection | utf8                       |
    | character_set_database   | utf8                       |
    | character_set_filesystem | binary                     |
    | character_set_results    | utf8                       |
    | character_set_server     | utf8                       |
    | character_set_system     | utf8                       |
    | character_sets_dir       | /usr/share/mysql/charsets/ |
    +--------------------------+----------------------------+
    8 rows in set (0.00 sec)

    mysql> SELECT @@global.tx_isolation;
    +-----------------------+
    | @@global.tx_isolation |
    +-----------------------+
    | READ-COMMITTED        |
    +-----------------------+
    1 row in set, 1 warning (0.00 sec)

已经是utf8编码,全局事务隔离等级也是READ-COMMITTED
##### 4.13 为配置confluence做准备
###### 创建相应的数据库和用户(这里数据库和用户都是叫confluence)

    create database confluence default character set utf8 collate utf8_bin;
    grant all on confluence.* to 'confluence'@'%' identified by '你的数据库登陆密码';
    flush privileges;
到这里Mysql基本配置好了

### 5、破解并配置confluence
##### 5.1 替换jar包
将```/opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.3.0.jar```这是6.8.0版本中的jar包具体的命名，不同版本可能不同，我们将这个jar包传到本地(windows)，然后重命名为```atlassian-extras-2.4.jar```命名必须按照这个。
打开破解工具进行破解，如下

![破解](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img4.png)

![破解完成](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img5.png)

将破解后的jar包上传到服务器，然后重命名为```atlassian-extras-decoder-v2-3.3.0.jar```替换掉```/opt/atlassian/confluence/confluence/WEB-INF/lib/```目录下的同名文件。

##### 5.2 汉化和Mysql连接
其中```atlassian-extras-decoder-v2-3.3.0.jar```文件是和license想关，```Confluence-6.8.0-m56-language-pack-zh_CN.jar```是confluence中文语言包[下载地址](https://translations.atlassian.com/dashboard/download?lang=zh_CN#/Bitbucket/5.8.0-rc3)，而```mysql-connector-java-5.1.3.jar```(如果你的Mysql版本不同，在下载次jar包时留一下支持mydql版本)是confluence连接mysql数据库相关的jar包。

    [root@bogon data]# mv Confluence-6.8.0-m56-language-pack-zh_CN.jar /opt/atlassian/confluence/confluence/WEB-INF/lib/
    [root@bogon data]# mv mysql-connector-java-5.1.3.jar /opt/atlassian/confluence/confluence/WEB-INF/lib/

##### 5.3 启动confluence

    [root@bogon data]# /etc/init.d/confluence1 start

    To run Confluence in the foreground, start the server with start-confluence.sh -fg
    executing using dedicated user: confluence1
    If you encounter issues starting up Confluence, please see the Installation guide at http://confluence.atlassian.com/display/DOC/Confluence+Installation+Guide

    Server startup logs are located in /opt/atlassian/confluence/logs/catalina.out
    ---------------------------------------------------------------------------
    Using Java: /opt/atlassian/confluence/jre//bin/java
    2018-10-01 04:05:33,248 INFO [main] [atlassian.confluence.bootstrap.SynchronyProxyWatchdog] A Context element for ${confluence.context.path}/synchrony-proxy is found in /opt/atlassian/confluence/conf/server.xml. No further action is required
    ---------------------------------------------------------------------------
    Using CATALINA_BASE:   /opt/atlassian/confluence
    Using CATALINA_HOME:   /opt/atlassian/confluence
    Using CATALINA_TMPDIR: /opt/atlassian/confluence/temp
    Using JRE_HOME:        /opt/atlassian/confluence/jre/
    Using CLASSPATH:       /opt/atlassian/confluence/bin/bootstrap.jar:/opt/atlassian/confluence/bin/tomcat-juli.jar
    Using CATALINA_PID:    /opt/atlassian/confluence/work/catalina.pid
    Tomcat started.

现在我们可以在浏览器(127.0.0.1:8090)中访问了，如果想在同一个局域网的其它机器上访问需要开放8090端口，关于Centos7关闭防火墙的教程可以参考[centos7关闭防火墙](https://www.jianshu.com/p/d6414b5295b8)

##### 5.4 在浏览器中配置
首页选择语言为中文：
![首页](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img6.png)

选择产品安装(下一步的插件我没选，直接下一步)
![安装](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img7.png)

拿到服务器ID使用激活工具生成授权码
![ID](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img8.png)
生成的授权码
![授权码](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img9.png)
粘贴到浏览器的授权码输入框中，点击下一步
这一步我们使用自己的数据库
![数据库配置](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img10.png)

这里使用字符串来配置数据库，这里的数据库和用户密码就是前面我们设置的。
![连接数据库](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img11.png)
这个时候需要等上一会，confluence会到数据库创建数据表。

##### 5.5 开始你的使用
出现下面界面就表示已经搭建好了，就可以使用了。
![开始使用](https://blog-1252420645.cos.ap-chengdu.myqcloud.com/article-imgs/centos7_confluence/img12.png)

### 6、END
