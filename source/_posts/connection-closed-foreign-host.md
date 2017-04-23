---
title: Connection closed by foreign host(已解决)
tags:
  - Connection closed by foreign host
  - xshell
id: 389
categories:
  - Linux
  - linux错误锦集
date: 2012-10-21 16:47:28
---

今天用xshell 去链接前段时间装的服务器，出现如下提示：
<pre class="brush: bash; gutter: true"> Connection closed by foreign host.</pre>
<span>
</span>

意思是 断开主机链接了，出现这种问题，跟你的IPTABLES，防火墙什么的都没关系。

造成这个原 因是因为原来连接到SSHD服务器进程的22端口，当你的客户端突然断开时，服务器端的TCP连接就处于一个半打开状态。当下一次同一客户机再次建立 TCP连接时，服务器检测到这个半打开的TCP连接，并向客户机回传一个置位RST的TCP报文，客户机就会显示connection closed by foreign host。
这是TCP协议本身的一个保护措施，并不是什么错误，你只要再重新连接服务器就能连上。

我用的是wifi，然后登录路由之后，断网，自动的重新链接即可了！

&nbsp;