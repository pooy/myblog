---
title: 如何添加Apache服务器用户验证
tags:
  - AllowOverride ALL
  - AllowOverride AuthConfig
  - apache服务器
id: 1294
categories:
  - Linux
  - 架构集群
date: 2013-04-28 13:08:13
---

apache服务器已经内置用户验证机制，大家只要适当的加以设置，便可以控制网站的某些部分要用户验证。大家只要跟着我一步步做下来就应该能轻松实现用户验证。

前期准备，必须已经安装apache.

**　　第1步：**

我们在/var/www(apache的主页根目录)下建立一个test目录
<pre class="brush: bash; gutter: false">mkdir /var/www/test</pre>
**　　第2步**

然后我们编辑httpd.conf

添加
<pre class="brush: bash; gutter: false">　　Alias /test&quot;/var/www/test&quot;
　　Options Indexes MultiViews
　　AllowOverride AuthConfig #表示进行身份验证
　　Order allow,deny
　　Allow from all
　　#AllowOverride AuthConfig 表示进行身份验证 这是关键的设置</pre>
&nbsp;

**　　第3步**

在/var/www/test创建.htaccess文件
<pre class="brush: bash; gutter: false">　　vi /var/www/test/.htaccess
　　AuthName &quot;frank share web&quot;
　　AuthType Basic
　　AuthUserFile /var/www/test/.htpasswd
　　require valid-user 
　　#AuthName 描述，随便写
　　#AuthUserFile /var/www/test/.htpasswd
　　#require valid-user 或者 require user frank 限制是所有合法用户还是指定用户
　　#密码文件推荐使用.htpasswd,因为apache默认系统对“.ht”开头的文件默认不允许外部读取，安全系数会高一点哦。</pre>
&nbsp;

**　　第4步**

就是创建apache的验证用户
<pre class="brush: bash; gutter: false">htpasswd -c /var/www/test/.htpasswd frank</pre>
#第一次创建用户要用到-c 参数 第2次添加用户，就不用-c参数

如果你们想修改密码，可以如下
<pre class="brush: bash; gutter: false">htpasswd -m .htpasswd frank</pre>
**　　第5步：**

ok，重启apache服务，然后访问 http://你的网站地址/test 如果顺利的话，应该能看到一个用户验证的弹出窗口，只要填入第4步创建的用户名和密码就行

后话，为了服务器的性能，一般不推荐使用**AllowOverride AuthConfig**或者**AllowOverride ALL**，因为这会使服务器会不断的去寻找.htaccess,从而影响服务器的效能，一般我们把一些后台管理界面或者其他特殊目录可能需要加验证这个需求。

（资料来自于互联网）