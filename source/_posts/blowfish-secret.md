---
title: 解决phpmyadmin配置文件现在需要绝密的短语密码（blowfish_secret）
tags:
  - blowfish_secret
  - centos配置phpmyadmin
  - phpmyadmin
  - 配置文件现在需要绝密的短语密码（blowfish_secret）
id: 797
categories:
  - Linux
  - linux错误锦集
  - Mysql
date: 2013-01-10 14:12:15
---

安装完成phpmyadmin之后，再在浏览器里输入:http://localhost/phpmyadmin这时能看到phpmyadmin的管理页面，不过会提示：“配置文件现在需要绝密的短密码（blowfish_secret）。”
<div><dl id="attachment_791"><dt>[![](http://www.pooy.net/wp-content/uploads/2013/01/phpmyadmin.jpg "phpmyadmin")](http://www.pooy.net/wp-content/uploads/2013/01/phpmyadmin.jpg)</dt><dd>phpmyadmin</dd></dl></div>
&nbsp;

解决办法有两种：（建议用第二种）

1、配置phpmyadmin下的config.inc.php  将cookie改为http
<pre class="brush: bash; gutter: true">vi /usr/share/phpmyadmin/config.inc.php  
[...]  
/* Authentication type */  
$cfg[&#039;Servers&#039;][$i][&#039;auth_type&#039;] = ‘cookie’;  
[...]</pre>
&nbsp;

再打开浏览器输入管理地址，这时会弹出登录窗口，输入用户名及密码及可。

&nbsp;

不过很不习惯，而且在进入管理界面后，选择登出时会再次弹出，让人感觉登出也要密码似的。

2、对比了一下ubuntu的phpmyadmin的配置，在ubuntu的config.inc.php里有这样一段配置
<pre class="brush: php; gutter: true">// Load secret generated on postinst
include(&#039;/var/lib/phpmyadmin/blowfish_secret.inc.php&#039;);</pre>
&nbsp;

再查看一下/var/lib/phpmyadmin/blowfish_secret.inc.php，只有一句
<pre class="brush: php; gutter: true">&amp;lt;?php
$cfg[&#039;blowfish_secret&#039;] = &#039;w1HM7AxcX5aQvutjVOyGdepy&#039;;</pre>
&nbsp;

那么CentOS下安装的phpmyadmin中的“$cfg['blowfish_secret'] =”语句在config.inc.php里
<pre class="brush: bash; gutter: true">vim /usr/share/phpmyadmin/config.inc.php</pre>
找到
<pre class="brush: php; gutter: true">$cfg[&#039;blowfish_secret&#039;] = &#039;&#039;; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */</pre>
在=后面加上任意字符
<pre class="brush: php; gutter: true">$cfg[&#039;blowfish_secret&#039;] = &#039;pooy&#039;; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */</pre>
pooy是我随意加上的字符

重启httpd再打开管理页面

还 是这个看着习惯点，不过再输入root及密码，系统提示我root@localhost密码错误：error 'Access denied for user 'root'@'localhost' (using password: NO)，总是进不去，于是清理了一下浏览器的cookie，再进就正常进入了