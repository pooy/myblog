---
title: linux下apache常用命令
tags:
  - apache
  - linux
  - linux下apache的指令
id: 57
categories:
  - Linux
  - 手册资料
date: 2011-07-16 14:12:23
---

经常要在[LAMP](http://www.pooy.net/category/network-programming/linux-apache-mysql-php "LAMP专栏")环境下做一些开发，自热而然的要跟这些指令打交道。下面是[璞玉](http://www.pooy.net "璞玉")整理的一些常用的指令。

&nbsp;

1)列出的是所有连接

<span>[ccei]netstat -tun[/ccei]</span>

2)列出的是apache所有连接
<span>[ccei]netstat -tun | grep ":80"[/ccei]</span>

3）统计80端口连接数

<span>[ccei]netstat -nat|grep-i"80"|wc -l[/ccei]</span>

4）统计httpd协议连接数

<span>[ccei]ps -ef|grep httpd|wc -l[/ccei]</span>

5）统计已连接上的，状态为“established'

<span>[ccei]netstat -na|grep ESTABLISHED|wc -l[/ccei]</span>

6）查出哪个IP地址连接最多，将其封了
<span>[ccei]netstat -na|grep ESTABLISHED|awk'{print$5}'|awk-F:'{print$1}'|sort|uniq-c|sort-r+0n[/ccei]</span>

<span>[ccei]netstat -na|grep SYN|awk'{print$5}'|awk-F:'{print$1}'|sort|uniq-c|sort-r+0n[/ccei]</span>

7)查看httpd进程数（即prefork模式下Apache能够处理的并发请求数）：
<span>[ccei]ps -ef | grep httpd | wc -l[/ccei]</span>

8)查看Apache的并发请求数及其TCP连接状态：
<span>[ccei]netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'[/ccei]</span>
或者

<span>[ccei]netstat -n | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"\t",state[key]}'[/ccei]</span>

返回结果示例：
<pre>[cce]
LAST_ACK 5
 SYN_RECV 30
 ESTABLISHED 1597
 FIN_WAIT1 51
 FIN_WAIT2 504
 TIME_WAIT 1057
[/cce]</pre>
说明:
SYN_RECV: 表示正在等待处理的请求数；
ESTABLISHED: 表示正常数据传输状态；
TIME_WAIT: 表示处理完毕，等待超时结束的请求数
CLOSED：无连接是活动的或正在进行
LISTEN：服务器在等待进入呼叫
SYN_RECV：一个连接请求已经到达，等待确认
SYN_SENT：应用已经开始，打开一个连接
ESTABLISHED：正常数据传输状态
FIN_WAIT1：应用说它已经完成
FIN_WAIT2：另一边已同意释放
ITMED_WAIT：等待所有分组死掉
CLOSING：两边同时尝试关闭
TIME_WAIT：另一边已初始化一个释放
LAST_ACK：等待所有分组死掉

如发现系统存在大量TIME_WAIT状态的连接，通过调整内核参数解决，
vim /etc/sysctl.conf
编辑文件，加入以下内容：
<span>[ccei]net.ipv4.tcp_syncookies = 1 net.ipv4.tcp_tw_reuse = 1 net.ipv4.tcp_tw_recycle = 1 net.ipv4.tcp_fin_timeout = 30[/ccei]</span>
然后执行 /sbin/sysctl -p 让参数生效。

net.ipv4.tcp_syncookies = 1 表示开启SYN cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；
net.ipv4.tcp_tw_reuse = 1 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
net.ipv4.tcp_tw_recycle = 1 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。
net.ipv4.tcp_fin_timeout 修改系統默认的 TIMEOUT 时间

下面附上TIME_WAIT状态的意义：

客户端与服务器端建立TCP/IP连接后关闭SOCKET后，服务器端连接的端口
状态为TIME_WAIT

是不是所有执行主动关闭的socket都会进入TIME_WAIT状态呢？
有没有什么情况使主动关闭的socket直接进入CLOSED状态呢？

主动关闭的一方在发送最后一个 ack 后
就会进入 TIME_WAIT 状态 停留2MSL（max segment lifetime）时间
这个是TCP/IP必不可少的，也就是“解决”不了的。

也就是TCP/IP设计者本来是这么设计的
主要有两个原因
1。防止上一次连接中的包，迷路后重新出现，影响新连接
（经过2MSL，上一次连接中所有的重复包都会消失）
2。可靠的关闭TCP连接
在主动关闭方发送的最后一个 ack(fin) ，有可能丢失，这时被动方会重新发
fin, 如果这时主动方处于 CLOSED 状态 ，就会响应 rst 而不是 ack。所以
主动方要处于 TIME_WAIT 状态，而不能是 CLOSED 。

TIME_WAIT 并不会占用很大资源的，除非受到攻击。

还有，如果一方 send 或 recv 超时，就会直接进入 CLOSED 状态

注：转自chinaunix.net