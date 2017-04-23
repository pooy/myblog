---
title: dedecms 验证码错误解决之道
tags:
  - DEDECMS
  - dedecms验证码
  - dedecms验证码出错
id: 416
categories:
  - DEDECMS
  - 开源软件
date: 2012-10-31 22:32:08
---

今天有个客户给我反映，系统验证码总是出错，登录不了，于是我立即去看看。系统是用[dedecms二次开发](http://www.pooy.net/down/dedecms5.7.doc "dedecms5.7数据库结构说明文档")的。

打开后台的地址，果然出现了那些问题，于是我打开了后台的/dede/login.php这个登录的处理页面，修改第67行
<pre class="brush: php; gutter: true">ResetVdValue();
ShowMsg($validate.&#039;验证码不正确!&#039;,&#039;login.php&#039;,0,1000);</pre>
&nbsp;

把验证码跟提示信息一起弹出来，发现没有问题。

于是检查[dedecms](http://www.pooy.net/category/network-programming/dedecms "dedecms ")的空间的大小，发现空间超了，如下图：

[caption id="attachment_417" align="aligncenter" width="343"][![](http://www.pooy.net/wp-content/uploads/2012/10/未命名.jpg "dedecms空间")](http://www.pooy.net/wp-content/uploads/2012/10/未命名.jpg) dedecms空间[/caption]

真有意思，还能超过限额大小，真够仁慈的，赶紧删除了一些缓存文件，跟一些没用的旧图。

唉，空间商真不够意思，磁盘满了都没个邮件提示。