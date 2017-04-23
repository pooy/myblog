---
title: Centos5.8 安装VPN 简要教程
tags:
  - centos vpn
  - openvpn
  - vpn
id: 1773
categories:
  - Linux
date: 2014-05-05 15:06:41
---

**CentOS 5.8 (32- or 64-bit)**

下载安装包：
<pre class="brush: bash; gutter: true">wget http://swupdate.openvpn.org/as/openvpn-as-1.8.4-CentOS5.x86_64.rpm</pre>
开始安装：
<pre class="brush: bash; gutter: true">rpm -i openvpn-as-1.8.4-CentOS5.x86_64.rpm</pre>
安装完成之后会出现：
<pre class="brush: bash; gutter: true">Access Server web UIs are available here:
Admin  UI: https://115.28.152.xxx:943/admin
Client UI: https://115.28.152.xxx:943/</pre>
<pre class="brush: bash; gutter: true">passwd openvpn</pre>
设置完**openvpn**的密码后。

登陆：**https://115.28.152.xxx:943/admin**

用户名为刚才设置的**openvpn**，密码为刚才自己设置的。

[![openvpn](http://www.pooy.net/wp-content/uploads/2014/05/openvpn.jpg)](http://www.pooy.net/wp-content/uploads/2014/05/openvpn.jpg)

服务器端一切正常，接下来开始安装客户端:

打开：**https://115.28.152.xxx:943/** 后，输入上面设置的密码：

[![openvpn_login](http://www.pooy.net/wp-content/uploads/2014/05/openvpn_login.jpg)](http://www.pooy.net/wp-content/uploads/2014/05/openvpn_login.jpg)进入之后就能看到：

[![openvpn_down](http://www.pooy.net/wp-content/uploads/2014/05/openvpn_down.jpg)](http://www.pooy.net/wp-content/uploads/2014/05/openvpn_down.jpg)点击下载openvpn-connect-1.8.3.347.ms，安装后：

[![openvpn_connect](http://www.pooy.net/wp-content/uploads/2014/05/openvpn_connect.jpg)](http://www.pooy.net/wp-content/uploads/2014/05/openvpn_connect.jpg)

输入完成之后就能连接到VPN.登陆ip138之后是不是发现IP已经变成了VPN的IP了？

到这里就安装完成。

**openvpn**在我本地的机器跟阿里云服务器上均测试，都能有效使用，且稳定。