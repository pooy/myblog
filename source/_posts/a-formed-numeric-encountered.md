---
title: PHP处理图片之二报A non well formed numeric value encountered错误处理
tags:
  - A non well formed numeric value encountered
  - php图片类
  - php处理图片
id: 63
categories:
  - PHP
  - 文件处理
date: 2011-08-17 18:21:15
---

关键字：A non well formed numeric value encountered,php处理图片,php图片类

####            今天在用php写一个图片在线系统的时候报错 A non well formed numeric value encountered 原因如下：

时间戳不是真正的int类型,这种经常出现在从数据库中提取出数据，但是数据不是int类型的，可能是varchar等等，这种问题常常出现在弱类型语言上！大家可以使用intval()函数将非格式良好的数据转换成良好的类型，这样就可以了！

&nbsp;

[![](http://www.pooy.net/wp-content/uploads/2012/07/1253679991vbyFzJel.png "PHP图片处理")](http://www.pooy.net/wp-content/uploads/2012/07/1253679991vbyFzJel.png)

&nbsp;

PHP图片处理源码
<div>      php处理图片之三[php处理图片函数](http://www.pooy.net/phpphp.html "php处理图片函数")可以去下载:http://www.pooy.net/phpphp.html</div>
<div></div>
<div></div>
<div></div>