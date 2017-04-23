---
title: 短信API调试遇到的一个问题
tags:
  - iptables
  - 短信API
id: 1734
categories:
  - 生活点滴
date: 2014-02-22 19:14:33
---

在测试机上使用第三方短信API的时候遇到了一个问题，就是CURL超时。本机测试啥问题都没有，然后就开始一步一步的排查。

1，查代码。（代码排除）

2，查服务器防火墙。

服务器防火墙使用的是[iptables](http://www.pooy.net/how-security-settings-for-iptables.html "iptables")。最后弄明白是因为iptables配置防火墙后本机无法访问外部网络。

OUTPUT和FORWORD没有任何限制，这有点想不通，只好google一把，发现有人也是遇到这样的问题，解决的办法是添加如下规则：
<pre class="brush: bash; gutter: true">/sbin/iptables -A INPUT -m state --state ESTABLISHED -j ACCEPT</pre>
为何在没有添加上述规则就不能通信？因为建立一个通信连接需要服务器端和客户端交互才能完成。

举例来说，从本机使用ssh客户端去登陆外部的ssh 服务器，假设使用端口为12345，那么本就使用tcp端口号12345向服务器22端口发送一个请求，这个属于OUTPUT，由于OUTPUT规则没有 任何限制，所以可以顺利到达服务器，服务器收到请求后，服务器会回应本机的tcp 12345端口，此时回应属于INPUT，如果INPUT中配置放行此规则，那么连接就无法完成，也即是本机无法和外部通信。

外部的网络那么多，总不能逐 条去配置INPUT规则，所以为了能访问外部网络，必须要配置上述规则。