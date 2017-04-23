---
title: 解决关于cacti登陆不跳转的问题
tags:
  - cacti
  - cacti登陆不跳转的问题
id: 1589
categories:
  - Cacti使用点滴
  - Linux
  - 防火墙
date: 2013-10-06 22:09:11
---

**关于cacti登陆不跳转的问题**

出现这个问题，一般就是**session**无法写入！所以就要给session文件夹足够的权限即可。

在[Cacti](http://www.pooy.net/category/linux-2/a-firewall/cacti%E7%9B%91%E6%8E%A7 "Cacti监控")根目录新建一个文件info.php,然后输入：
<pre class="brush: php; gutter: false">&lt;?php
phpinfo();
?&gt;</pre>
&nbsp;

然后再浏览器下输入：http://你的域名/info.php 。就能看到php的配置信息，其中有一行是session.save.path,就是记录session存放的文件夹地址，[璞玉](http://www.pooy.net/ "璞玉")的如图所示：

[caption id="" align="alignnone" width="633"]![](http://www.pooy.net/wp-content/uploads/2013/11/cacti-nologin.jpg "解决cacti登陆不跳转") 解决cacti登陆不跳转[/caption]
<pre class="brush: bash; gutter: false">chmod 0777  /data/www/session</pre>
到这里[**Cacti**](http://www.pooy.net/category/linux-2/a-firewall/cacti%E7%9B%91%E6%8E%A7 "Cacti监控")就可以登陆后跳转了。