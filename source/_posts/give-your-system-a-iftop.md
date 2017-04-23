---
title: 给你的系统装个iftop吧！
tags:
  - iftop
  - iftop安装方法
  - iftop指令
  - linux
id: 639
categories:
  - Linux
  - 开源软件
  - 手册资料
date: 2012-05-25 21:22:46
---

<div>

在类linux系统中可以使用top查看系统资源、进程、内存占用等信息。查看网络状态可以使用netstat、nmap等工具。若要查看实时的网络流量，监控TCP/IP连接等，则可以使用[iftop](http://www.vpser.net/manage/iftop.html)。

## 一、iftop是什么？

iftop是类似于[linux](http://www.pooy.net/tag/linux "linux")下面top的实时流量监控工具。

官方网站：[http://www.ex-parrot.com/~pdw/iftop/](http://www.ex-parrot.com/%7Epdw/iftop/)

## 二、iftop有什么用？

iftop可以用来监控网卡的实时流量（可以指定网段）、反向解析IP、显示端口信息等，详细的将会在后面的使用参数中说明。

## 三、安装iftop

### **安装方法1、**编译安装

如果采用编译安装可以到[iftop官网](http://www.ex-parrot.com/%7Epdw/iftop/)下载最新的源码包。

安装前需要已经安装好基本的编译所需的环境，比如make、gcc、autoconf等。安装iftop还需要安装libpcap和libcurses。

**CentOS上安装所需依赖包：**
<pre class="brush: bash; gutter: true">yum install flex byacc  libpcap ncurses ncurses-devel</pre>
**Debian上安装所需依赖包：**
<pre class="brush: bash; gutter: true">apt-get install flex byacc  libpcap0.8 libncurses5</pre>
下载iftop
> <pre class="brush: bash; gutter: true">wget http://www.ex-parrot.com/pdw/iftop/download/iftop-0.17.tar.gz
> tar zxvf iftop-0.17.tar.gz
> cd iftop-0.17
> ./configure
> make &amp;&amp; make install</pre>

### 安装方法2：(懒人办法，最简单)

直接省略上面的步骤

CentOS系统运行：
<pre class="brush: bash; gutter: true">yum install iftop</pre>
Debian系统 运行：
<pre class="brush: bash; gutter: true">apt-get install iftop</pre>

## 四、运行iftop

直接运行： iftop

效果如下图：

[caption id="attachment_640" align="aligncenter" width="675"][![](http://www.pooy.net/wp-content/uploads/2012/11/iftop.jpg "iftop 教程")](http://www.pooy.net/wp-content/uploads/2012/11/iftop.jpg) iftop 教程[/caption]

## 五、相关参数及说明

### 1、iftop界面相关说明

界面上面显示的是类似刻度尺的刻度范围，为显示流量图形的长条作标尺用的。

中间的&lt;= =&gt;这两个左右箭头，表示的是流量的方向。
<pre class="brush: c; gutter: true">TX：发送流量
 RX：接收流量
 TOTAL：总流量
 Cumm：运行iftop到目前时间的总流量
 peak：流量峰值
 rates：分别表示过去 2s 10s 40s 的平均流量</pre>

### 2、iftop相关参数

### 常用的参数

<pre class="brush: c; gutter: true">-i设定监测的网卡，如：# iftop -i eth1
-B 以bytes为单位显示流量(默认是bits)，如：# iftop -B
-n使host信息默认直接都显示IP，如：# iftop -n
-N使端口信息默认直接都显示端口号，如: # iftop -N
-F显示特定网段的进出流量，如# iftop -F 10.10.1.0/24或# iftop -F 10.10.1.0/255.255.255.0
-h（display this message），帮助，显示参数信息
-p使用这个参数后，中间的列表显示的本地主机信息，出现了本机以外的IP信息;
-b使流量图形条默认就显示;
-f这个暂时还不太会用，过滤计算包用的;
-P使host信息及端口信息默认就都显示;
-m设置界面最上边的刻度的最大值，刻度分五个大段显示，例：# iftop -m 100M</pre>

### 进入iftop画面后的一些操作命令(注意大小写)

<pre class="brush: c; gutter: true">按h切换是否显示帮助;
按n切换显示本机的IP或主机名;
按s切换是否显示本机的host信息;
按d切换是否显示远端目标主机的host信息;
按t切换显示格式为2行/1行/只显示发送流量/只显示接收流量;
按N切换显示端口号或端口服务名称;
按S切换是否显示本机的端口信息;
按D切换是否显示远端目标主机的端口信息;
按p切换是否显示端口信息;
按P切换暂停/继续显示;
按b切换是否显示平均流量图形条;
按B切换计算2秒或10秒或40秒内的平均流量;
按T切换是否显示每个连接的总流量;
按l打开屏幕过滤功能，输入要过滤的字符，比如ip,按回车后，屏幕就只显示这个IP相关的流量信息;
按L切换显示画面上边的刻度;刻度不同，流量图形条会有变化;
按j或按k可以向上或向下滚动屏幕显示的连接记录;
按1或2或3可以根据右侧显示的三列流量数据进行排序;
按&lt;根据左边的本机名或IP排序;
按&gt;根据远端目标主机的主机名或IP排序;
按o切换是否固定只显示当前的连接;
按f可以编辑过滤代码，这是翻译过来的说法，我还没用过这个！
按!可以使用shell命令，这个没用过！没搞明白啥命令在这好用呢！
按q退出监控。</pre>

## 六、常见问题

1、make: yacc: Command not found
<pre class="brush: bash; gutter: true">make: *** [grammar.c] Error 127</pre>
解决方法：apt-get install byacc   /   yum install byacc

2、configure: error: Curses! Foiled again!
(Can't find a  [meatglue.org](http://4seohunt.biz/rep/meatglue.org) curses  library supporting mvchgat.)
Consider installing n [meatglue.org](http://4seohunt.biz/rep/meatglue.org) curses .

解决方法：
<pre class="brush: bash; gutter: true">apt-get install libncurses5-dev  /    yum  install ncurses-devel

用上iftop后再也不担心自己的流量去哪儿了。</pre>
</div>