---
title: dedecms 自定义标签使用详解
tags:
  - DEDECMS
  - dedecms 自定义标签
  - dedecms 自定义标签使用详解
id: 304
categories:
  - DEDECMS
  - 开源软件
date: 2012-08-23 23:11:36
---

前段时间写了一篇关于[dedecms采集的一些采集规律](http://www.pooy.net/dedecms.html "详细阅读 dedecms采集的一些采集规律")，今天给大家介绍dedecms 自定义标签使用。

1， 在include/tablib当中新建一个标签文件

2，在此标签文件中， 创建一个Lib_标签名的函数，函数有2个参数，第一个是代表当前标签信息的对象，第二个是代表当前模板内数据信息的变量。

3，在函数中处理标签属性

4，处理标签所需要的数据

5，处理底层模板使用dedetagparse对象解析。

大家可以参考demotag.lib.php完成自定义标签的处理。调试在后台的 模版=》全局标记测试 里面测试。不明白的可以一起交流！