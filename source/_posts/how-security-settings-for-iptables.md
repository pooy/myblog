---
title: 怎么安全设置iptables
tags:
  - iptables
  - 一步搞定iptables
  - 如何设置iptables
  - 怎么安全设置iptables
id: 554
categories:
  - Linux
  - 手册资料
  - 防火墙
date: 2012-11-21 23:00:02
---

之前写了一篇[ 《iptables详讲 菜鸟的突破 ](http://www.pooy.net/iptables.html "iptables详讲 菜鸟的突破")》 应该能对iptables，有一定的了解了吧。

不过在日常工作中，去设置这个还是要小心，要不然就把自己挡在门外了。

&nbsp;

如何安全的设置[iptables](http://www.pooy.net/iptables.html "iptables")呢？

首先，准备一个清除或者备份iptables的脚本。放在[crontab](http://www.pooy.net/ubuntu-crontab.html "crontab")或者[screen](http://www.pooy.net/linux-screen.html "linux screen 命令大全 ")里面，让他定时执行。
<div>例如/home/pooy/iptables.clear文件</div>
脚本如下：
<pre class="brush: bash; gutter: true">#!/bin/sh
# IPTables Location - adjust if needed
IPT=&quot;/sbin/iptables&quot;
IPTS=&quot;/sbin/iptables-save&quot;
IPTR=&quot;/sbin/iptables-restore&quot;
####################################
#
# Flush Any Existing Rules or Chains
#
echo &quot;Flushing Tables ...&quot;
# Reset Default Policies
$IPT -P INPUT ACCEPT
$IPT -P FORWARD ACCEPT
$IPT -P OUTPUT ACCEPT
$IPT -t nat -P PREROUTING ACCEPT
$IPT -t nat -P POSTROUTING ACCEPT
$IPT -t nat -P OUTPUT ACCEPT
$IPT -t mangle -P PREROUTING ACCEPT
$IPT -t mangle -P OUTPUT ACCEPT
# Flush all rules
$IPT -F
$IPT -t nat -F
$IPT -t mangle -F
# Erase all non-default chains
$IPT -X
$IPT -t nat -X
$IPT -t mangle -X
echo &quot;Done.&quot;

假如我是放在crontab里面的，我就十分钟去执行一次！</pre>
<div>
<pre class="brush: bash; gutter: true">*/10 * * * * /home/stef/iptables.clear &gt; /dev/null</pre>
</div>
<pre class="brush: bash; gutter: true">然后我就开始去写我的iptables规则，就不怕被“意外”拒之门外了。嘿~</pre>