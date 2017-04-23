---
title: 您的apache安装mod_pagespeed没？
tags:
  - apache pagespeed
  - apache安装mod_pagespeed
  - google pagespeed
  - mod_pagespeed下载
  - pagespeed firefox
id: 634
categories:
  - Linux
  - 架构集群
date: 2012-03-24 16:45:49
---

[caption id="attachment_635" align="aligncenter" width="267"][![](http://www.pooy.net/wp-content/uploads/2012/11/TM截图未命名.jpg "page speed")](http://www.pooy.net/wp-content/uploads/2012/11/TM截图未命名.jpg) page speed[/caption]

mod_pagespeed模块 在上次我们网站整体优化中就包括启用谷歌提供的[mod_pagespeed](http://www.pooy.net/tag/mod_pagespeed "mod_pagespeed")模块。

mod_pagespeed诞生是在2009年六月。Google 为开发者提供了一个可以给出相关网站优化建议的工具 Page Speed，但是有了建议后的后续执行工作也是很麻烦的。贴心的 Google 特为懒惰型 MJJ们 提供了傻瓜化解决方案：mod_pagespeed。

mod_pagespeed直接把这个本来是要靠浏览器自身来优化的任务，给了服务端。

该模块可以有效将网页加载速度提高50%+，Google这款加速模块简单的解决了许多复情况的问题：

1.  加速模块可以自行对网络传输的html字节优化及对图象，css进入压缩优化传输；
2.  js的自动压缩；
3.  智能缓存是一大亮点，它可以自动智能缓存，加速下载。
4.  直接开启模块即可，不需要过多设置；
下面以[ubuntu](http://www.pooy.net/tag/ubuntu "ubuntu")系统[ apache](http://www.pooy.net/tag/apache "apache")2.2为例：

*   下载32-bit
<table width="95%" border="0" cellspacing="0" cellpadding="6" align="center">
<tbody>
<tr>
<td bgcolor="#ddedfb">wegt https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-beta_current_i386.deb</td>
</tr>
</tbody>
</table>

*   下载64-bit
<table width="95%" border="0" cellspacing="0" cellpadding="6" align="center">
<tbody>
<tr>
<td bgcolor="#ddedfb">wegt https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-beta_current_amd64.deb</td>
</tr>
</tbody>
</table>
安装 mod_pagespeed ：
<table width="95%" border="0" cellspacing="0" cellpadding="6" align="center">
<tbody>
<tr>
<td bgcolor="#ddedfb">sudo dpkg-i mod-pagespeed*.deb

apt-get-f install</td>
</tr>
</tbody>
</table>
删除下载文件mod_pagespee
<table width="95%" border="0" cellspacing="0" cellpadding="6" align="center">
<tbody>
<tr>
<td bgcolor="#ddedfb">rm mod-pagespeed*.deb</td>
</tr>
</tbody>
</table>
修改域名并重启apache
<pre class="brush: bash; gutter: true">sudo /etc/init.d/apache2 restart</pre>
注：意其中的mod-pagespeed*.deb为对应模块版本文件名

设置pagespeed.conf ：
<pre class="brush: bash; gutter: true">nano /etc/apache2/mods-available/pagespeed.conf</pre>
修改域名并重启apache
<pre class="brush: bash; gutter: true">sudo /etc/init.d/apache2 restart</pre>
刷新网站几次，确保安装成功.

在实际测试中提速很明显，不过也有一定的兼容问题，比如在IE8.0下面与jquery的不兼容，比如在IE7.0和IE6.0图片不能延迟加载。这些问题之前已经写过一篇《[解决 pagespeed 浏览器不兼容问题 ](http://www.pooy.net/pagespeed.html "解决 pagespeed 浏览器不兼容问题")》。欢迎查看。