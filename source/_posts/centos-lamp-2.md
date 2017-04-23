---
title: Centos配置LAMP环境及常见问题解决
tags:
  - centos
  - centos下安装LAMP环境，
  - centos配置LAMP
  - LAMP
id: 790
categories:
  - Linux
  - Mysql
  - 手册资料
date: 2012-04-10 14:03:18
---

以前在Ubuntu上安装过LAMP，一直没在CentOS上装过，认为过这两个好像安装都一样，很方便。

第一步：安装apache mysql php

#yum install httpd httpd-devel mysql mysql-server mysql-devel php -y

//安装apacher服务器、apacher所需的库和包含文件、MySQL服务器、MySQL所需的库和包含文件、PHP

系统提示：
<pre class="brush: xhtml; gutter: true">Installing
httpd-devel、mysql-server、mysql、mysql-devel、php
installing for dependencies:
apr-devel、apr-util-devel、cyrus-sasl-devl、db4-devel、e2fsprogs-devel、 expat-devel、gcc、glibc-debel、glibc-headers、kernel-headers、keyutils-libs- devel、krb5-devel、libselinux-devel、libsepol-devel、openldap-devel、openssl- devel、perl-DBD-MySQL、perl-DBI、php-cli、php-common、zlib-devel、</pre>
&nbsp;

&nbsp;

第二步：配置MySQL

创建mysql启动链接
<pre class="brush: bash; gutter: true">chkconfig --levels 235 mysqld on                 //这样mysql会随着系统启动而启动</pre>
&nbsp;

启动mysql
<pre class="brush: bash; gutter: true">#etc/init.d/mysqld start</pre>
给root设置密码：
<pre class="brush: bash; gutter: true"># mysql_secure_installation</pre>
系统提示：
<pre class="brush: bash; gutter: true">In order to log into MySQL to secure it, we&#039;ll need the current
password for the root user.  If you&#039;ve just installed MySQL, and
you haven&#039;t set the root password yet, the password will be blank,
so you should just press enter here.
Enter current password for root (enter for none):   （一般刚装上mysql，root没有密码，在此直接Enter）
Setting the root password ensures that nobody can log into the MySQL
root user without the proper authorisation.
Set root password? [Y/n] Y
New password: 12345678    //设定新密码
Re-enter new password:12345678   //再次确认密码</pre>
&nbsp;

之后一路Enter就可以了

第三步：测试apache和php

创建httpd启动链接
<pre class="brush: bash; gutter: true">chkconfig --levels 235 httpd on</pre>
启动apache
<pre class="brush: bash; gutter: true">/etc/init.d/httpd start</pre>
apache测试：在浏览器里输入服务器IP，打开后看到apache那经典的页面，OK！apache正常运行了（如开了防火墙，不要忘记在防火墙放行www）。

PHP测试：创建个php.info
<pre class="brush: bash; gutter: true">vim /var/www/html/info.php</pre>
<pre class="brush: php; gutter: true">&lt;?
phpinfo();
?&gt;</pre>
&nbsp;

然后在浏览器里输入http://localhost/info.php应该能看到测试页面

第四步：让PHP支持[mysql](http://http://www.pooy.net/tag/centos-%E5%AE%89%E8%A3%85mysql "mysql")
<pre class="brush: bash; gutter: true">yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc  php-mbstring</pre>
系统提示：（安装以下软件包）
<pre class="brush: bash; gutter: true">Installed:
php-gd.i386 0:5.1.6-27.el5_5.3               php-imap.i386 0:5.1.6-27.el5_5.3          php-ldap.i386 0:5.1.6-27.el5_5.3
php-mbstring.i386 0:5.1.6-27.el5_5.3         php-mysql.i386 0:5.1.6-27.el5_5.3         php-odbc.i386 0:5.1.6-27.el5_5.3
php-pear.noarch 1:1.4.9-6.el5                php-xml.i386 0:5.1.6-27.el5_5.3           php-xmlrpc.i386 0:5.1.6-27.el5_5.3</pre>
&nbsp;

安装好后再重启httpd
<pre class="brush: bash; gutter: true">#/etc/init.d/httpd restart</pre>
再在浏览器里输入http://localhost/info.php应能看到mysql的模块

第五步：安装phpmyadmin

在centos里phpmyadmin无法用yum install来安装，先要加入
<pre class="brush: bash; gutter: true">wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.i386.rpm</pre>
&nbsp;
<pre class="brush: bash; gutter: true">rpm -Uvh rpmforge-release-0.5.2-2.el5.rf.i386.rpm</pre>
然后再
<pre class="brush: bash; gutter: true">yum install phpmyadmin</pre>
系统提示将装安装下列程序包
<pre class="brush: bash; gutter: true">Installed:
  phpmyadmin.noarch 0:2.11.11.3-2.el5.rf                                                                                       
Dependency Installed:
  libmcrypt.i386 0:2.5.8-4.el5.centos                            php-mcrypt.i386 0:5.1.6-15.el5.centos.1</pre>
&nbsp;

修改phpmyadmin.conf让用户可以远程登录
<pre class="brush: bash; gutter: true">vim /etc/httpd/conf.d/phpmyadmin.conf</pre>
将下面的语句全部注释掉
<pre class="brush: bash; gutter: true">&lt;Directory &quot;/usr/share/phpmyadmin&quot;&gt;
Order Deny,Allow
Deny from all
Allow from 127.0.0.1
&lt;/Directory&gt;</pre>
如果出现：[phpmyadmin配置文件现在需要绝密的短语密码（blowfish_secret）](http://www.pooy.net/blowfish-secret.html "blowfish-secret")

请移步：[http://www.pooy.net/blowfish-secret.html](http://www.pooy.net/blowfish-secret.html)

[解决Apache 2 Test Page powered by CentOS问题](http://www.pooy.net/928.html "解决Apache 2 Test Page powered by CentOS问题")

&nbsp;

&nbsp;

&nbsp;