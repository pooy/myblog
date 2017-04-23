---
title: 详解centos安装lamp环境
tags:
  - centos
  - centos 安装apache
  - centos 安装mysql
  - centos 安装php
  - centos 安装phpmyadmin
id: 126
categories:
  - Linux
  - 手册资料
date: 2012-07-16 16:08:06
---

上次我们说到了[centOS 卸载之前的 php-mysql-apache](http://www.pooy.net/centos-mysql-apache-php.html "centos卸载之前的php-mysql-apache")

这次璞玉细说[centos 安装lamp环境](http://www.pooy.net/centos-lamp.html "centos 安装LAMP环境")。怎么centos 安装lamp环境才不出错。璞玉用的是内网的ip：192.168.3.72 其中配置过虚拟机。

下面开始：

1\. 安装MySQL 5.0

安装MySQL 5.0，我们在终端中执行如下命令，

yum install mysql mysql-server
CentOS中安装完MySQL默认是不启动的，而且系统随机启动项里也不会自动添加mysqld的项，不过，还好这些都不是什么问题，简单的两个命令就能搞定它们： chkconfig --levels 235 mysqld on
/etc/init.d/mysqld start
使用过Debian/Ubuntu的朋友可能已经注意到，CentOS下安装MySQL不像Debian/Ubuntu那样，安装过程中就要求给 mysql的root用户设定密码。而在CentOS中，安装完毕后，我们还要使用下面的命令给mysql的root用户设定密码： mysqladmin -u root password yourrootsqlpassword
mysqladmin -h server1.example.com -u root password yourrootsqlpassword这一步一定要注意，任何人都有可能进入你的mysql数据库哦。。。

2\. 安装Apache2

Apache2已经包含在CentOS软件包中了，因此使用下面的命令就能轻松安装它了： yum install httpd
现在，设置Apache在系统启动中运行， chkconfig --levels 235 httpd on
立即启动之， /etc/init.d/httpd start
OK，这个时候就可以使用浏览器打开[http://192.168.3.72](http://192.168.0.100/)了，你可以看到CentOS的Apache的测试页面.

CentOS中，Apache的站点默认根目录(document root)位于 /var/www/html，配置文件位于 /etc/httpd/conf/httpd.conf，   ServerName localhost还有一些其他的配置文件，都不许在 /etc/httpd/conf.d/ 文件夹下。

3\. 安装PHP5

既然是“快速安装”，文中的步骤都是以快速且最小化安装为准。安装PHP5： yum install php
然后，重新启动Apache： /etc/init.d/httpd start
4\. 测试PHP5，查看PHP5安装的详细信息

测试PHP且要查看PHP5安装的相关信息最常用的做法是，在Apache站点根目录（/var/www/html）里新建一个名为 infor.php 的PHP程序文件，vi /var/www/html/info.php 代码如下：

&lt;?php
phpinfo();
?&gt;
PHP中phpinfo()这个函数就是用来显示PHP的具体信息的，在浏览器在打开[http://192.168.3.72/info.php](http://192.168.0.100/info.php)：
从图中我们能看到，PHP5已经能正常工作了，继续往下看，可以发现，常用的功能模块都已经启动了。当然，MySQL此时还没有现身，因为我们还没有为PHP5安装MySQL支持。

5\. 为PHP5安装MySQL支持

为了让PHP支持MySQL，我们还要安装php-mysql安装包。安装php-mysql软件包之前，我们回过头看看，我们所需要的PHP支持模块是不是都安装了呢？CentOS也提供了对软件包进行查找的命令： yum search php
复制代码使用上面这个命令，可以检索出所有php相关的软件包，从中选出我们需要的加以安装： yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring
安装完成后，不要忘了重启一下Apache2： /etc/init.d/httpd restart
现在，重新打开[http://192.168.3.72/info.php](http://192.168.0.100/info.php)页面，就可以看到mysql的支持项了：

&nbsp;

[caption id="attachment_132" align="alignnone" width="728"][![](http://www.pooy.net/wp-content/uploads/2012/07/centos-php.jpg "centos-php")](http://www.pooy.net/wp-content/uploads/2012/07/centos-php.jpg) centos-php[/caption]

&nbsp;

&nbsp;

6,**安装 phpMyAdmin管理数据库**

&nbsp;

CentOS系统中启用RPMForge软件库安装phpMyAdmin：

**64****位系统使用如下命令**：

wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm

rpm -Uhv rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm

**32****位系统使用如下命令:**

wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.i386.rpm

rpm -Uhv rpmforge-release-0.5.2-2.el5.rf.i386.rpm

现在可以安装phpMyAdmin如下命令：

yum install phpmyadmin

现在配置phpMyAdmin。需要改变Apache的配置，使phpMyAdmin不只是从本地主机连接（通过注释掉）：

vi /etc/httpd/conf.d/phpmyadmin.conf

&nbsp;

&nbsp;

找到相似内容代码，作如下配置：

#

# Web application to manage MySQL

#

#

# Order Deny,Allow

# Deny from all

# Allow from 127.0.0.1

#

Alias /phpmyadmin /usr/share/phpmyadmin

Alias /phpMyAdmin /usr/share/phpmyadmin

Alias /mysqladmin /usr/share/phpmyadmin

下一步，我们改变在phpMyAdmin认证cookie为HTTP：

vi /usr/share/phpmyadmin/config.inc.php

找到相似内容代码，作如下配置：

&nbsp;

[caption id="attachment_131" align="alignnone" width="551"][![centos-phpmyadmin](http://www.pooy.net/wp-content/uploads/2012/07/centos-phpmyadmin2.jpg "centos-phpmyadmin2")](http://www.pooy.net/wp-content/uploads/2012/07/centos-phpmyadmin2.jpg) centos-phpmyadmin[/caption]

/* Authentication type */

$cfg['Servers'][$i]['auth_type'] = ‘http’;

重启Apache:

/etc/init.d/httpd restart

访问下phpMyAdmin，地址：http://192.168.3.72/phpmyadmin/

&nbsp;

&nbsp;

[caption id="attachment_130" align="alignnone" width="886"][![](http://www.pooy.net/wp-content/uploads/2012/07/centos-phpmyadmin-1024x571.jpg "centos-phpmyadmin")](http://www.pooy.net/wp-content/uploads/2012/07/centos-phpmyadmin.jpg) centos安装phpmyadmin成功[/caption]

相关软件连接：

Apache: http://httpd.apache.org/

PHP: http://www.php.net/

MySQL: http://www.mysql.com/

CentOS: http://www.centos.org/

phpMyAdmin: http://www.phpmyadmin.net/

&nbsp;