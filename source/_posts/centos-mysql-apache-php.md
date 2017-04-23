---
title: centOS 卸载之前的 php-mysql-apache
tags:
  - centos
  - centOS卸载apache
  - centOS卸载mysql
  - centOS卸载php
id: 123
categories:
  - Linux
  - linux错误锦集
  - 手册资料
date: 2012-07-14 15:47:14
---

[caption id="attachment_124" align="aligncenter" width="370"][![](http://www.pooy.net/wp-content/uploads/2012/07/centos.jpg "centos 卸载mysql-php-apache")](http://www.pooy.net/wp-content/uploads/2012/07/centos.jpg) centos 卸载mysql-php-apache[/caption]

centOS 卸载之前的 php-mysql-apache

安装完centOS系统之后不要急于去架设LAMP环境,因为系统自身就有可能自己安装这些软件。

首先需要去卸载，提到卸载可能很多人会想到，直接rpm -qa |grep &lt;软件名&gt; 查找，查找到之后就rpm -e |grep &lt;软件名&gt; 删除！璞玉之前也是这么做的。没什么效果，而是报错！最后查询得知得如下操作才管用！

卸载mysql：rpm -qa | grep mysq;

&nbsp;

**[root@localhost mysql]# rpm -qa|grep -i mysql**

**mysql-5.0.45-7.el5**

**[root@localhost mysql]# rpm -ev mysql-5.0.45-7.el5**

**error: Failed dependencies:**

**libmysqlclient.so.15 is needed by (installed) dovecot-1.0.7-2.el5.i386**

**libmysqlclient.so.15(libmysqlclient_15) is needed by (installed) dovecot-1.0.7-2.el5.i386**

** **

**运行：# rpm -ev dovecot-1.0.7-2.el5.i386**

**然后运行：rpm -e mysql-5.0.45-7.el5**

**搞定！**

** **

卸载自带的php：

[root@localhost mysql]# rpm -qa |grep php

php-common-5.1.6-20.el5

[root@localhost mysql]# rpm -e php-common-5.1.6-20.el5

error: Failed dependencies:

php-common = 5.1.6-20.el5 is needed by (installed) php-cli-5.1.6-20.el5.i386

php-common = 5.1.6-20.el5 is needed by (installed) php-5.1.6-20.el5.i386

php-common = 5.1.6-20.el5 is needed by (installed) php-ldap-5.1.6-20.el5.i386

&nbsp;

运行：# yum -y remove php-common-5.1.6-20.el5

Setting up Remove Process

Resolving Dependencies

--&gt; Running transaction check

---&gt; Package php-common.i386 0:5.1.6-20.el5 set to be erased

--&gt; Processing Dependency: php-common = 5.1.6-20.el5 for package: php-cli

--&gt; Processing Dependency: php-common = 5.1.6-20.el5 for package: php

--&gt; Processing Dependency: php-common = 5.1.6-20.el5 for package: php-ldap

--&gt; Running transaction check

---&gt; Package php.i386 0:5.1.6-20.el5 set to be erased

--&gt; Processing Dependency: php for package: piranha

---&gt; Package php-ldap.i386 0:5.1.6-20.el5 set to be erased

---&gt; Package php-cli.i386 0:5.1.6-20.el5 set to be erased

--&gt; Running transaction check

---&gt; Package piranha.i386 0:0.8.4-9.3.el5 set to be erased

--&gt; Finished Dependency Resolution

&nbsp;

Dependencies Resolved

&nbsp;

=============================================================================

Package                 Arch       Version          Repository        Size

=============================================================================

Removing:

php-common              i386       5.1.6-20.el5     installed         396 k

Removing for dependencies:

php                     i386       5.1.6-20.el5     installed         2.9 M

php-cli                 i386       5.1.6-20.el5     installed         5.1 M

php-ldap                i386       5.1.6-20.el5     installed          43 k

piranha                 i386       0.8.4-9.3.el5    installed         5.0 M

&nbsp;

Transaction Summary

=============================================================================

Install      0 Package(s)

Update       0 Package(s)

Remove       5 Package(s)

&nbsp;

Downloading Packages:

Running rpm_check_debug

Running Transaction Test

Finished Transaction Test

Transaction Test Succeeded

Running Transaction

Erasing   : php-common                   ######################### [1/5]

Erasing   : php                          ######################### [2/5]

Erasing   : php-ldap                     ######################### [3/5]

Erasing   : piranha                      ######################### [4/5]

Erasing   : php-cli                      ######################### [5/5]

&nbsp;

Removed: php-common.i386 0:5.1.6-20.el5

Dependency Removed: php.i386 0:5.1.6-20.el5 php-cli.i386 0:5.1.6-20.el5 php-ldap.i386 0:5.1.6-20.el5 piranha.i386 0:0.8.4-9.3.el5

Complete!

[root@localhost mysql]# rpm -qa |grep php

[root@localhost mysql]#

呵呵，是不是 [centOS 卸载之前的 php-mysql-apache](http://www.pooy.net/centos-mysql-apache-php.html) 搞定了呢？

下次璞玉细说centos 安装lamp环境。怎么centos 安装lamp环境才不出错。

&nbsp;

** **