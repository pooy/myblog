---
title: centos FTP服务器的架设和配置
tags:
  - centos
  - centos ftp服务器
  - centos ftp配置
  - centos 配置vsftpd
id: 985
categories:
  - FTP下载周边
  - Linux
date: 2012-03-26 15:02:04
---

<div>

文件传输协议 (FTP) 是一个 TCP 协议，用于在计算机之间上传和下载文件。FTP 工作在客户端/服务器模式下。服务器组件被称为 FTP 守护程序。它持续不断地临听来自远程客户端的 FTP 请求。当一个请求到达时，它管理登录和建立连接。在整个会话期间它执行 FTP 客户端发送来的任何命令。 可以通过两种方式来管理 FTP 服务器的访问：

o 匿名

o 授权

在匿名模式中，远程客户端可以使用 “anonymous” 或 “ftp” 缺省用户帐号并通过发送一个邮件地 址做为密码来访问 FTP 服务器。在授权模式下一个用户必须拥有帐号和密码。用户所访问 FTP 服务器中目录和文件的权限是根据登录时所用帐号来定义的。一般来说，FTP 守护程序将隐藏在 FTP 服务器的根目录中并将其改到 FTP 家目录。这样就可以向远程传话隐藏文件系统的其他部分。

vsftpd - FTP 服务器安装

* vsftpd 是可在 Centos中使用的 FTP 守护程序之一。它在安装、设置和维护方面十分方便。要安装 vsftpd 您可以使用下列命令：
<pre class="brush: bash; gutter: true"> 　sudo yum  install vsftpd</pre>
vsftpd - FTP 服务器配置

* 你可以编辑 vsftpd 配置文件，/etc/vsftpd/vsftpd.conf，来配置缺省设置。

anonymous_enable=YES：是否允许匿名ftp，如否，则选择NO；

local_enable=YES：是否允许本地用户登陆；

local_umask=022：设置本地用户的文件掩码为缺省022，默认值为077；

anon_upload_enable=YES：是否允许匿名上传文件；

anon_mkdir_write_enable=YES：是否允许匿名用户有创建目录的权利；

dirmessage_enable=YES：是否显示目录说明文件，缺省是YES，但需要手工创建.message文件；

xferlog_enable=YES：是否记录ftp传输过程；

connect_from_port_20=YES：是否确信端口传输来自20（ftp-data）；

chown_username=username：是否改变上传文件的属主，如果需要，则输入一个系统用户名，可以把上传的文件都改成root属主；

xferlog_file=/var/log/vsftpd.log：ftp传输日志的路径和名字缺省是/var/log/vsftpd.log；

xferlog_std_format=YES：是否使用标准的ftp xferlog模式；

idle_session_timeout=600：设置缺省的断开不活跃会话时间；

data_connection_timeout=120：设置数据传输超时时间；

nopriv_user=ftpsecure：运行vsftpd需要的非特权系统用户，缺省是nobody；

ascii_upload_enable=YES：是否使用ASCII方式上传文件；

ascii_download_enable=YES：是否使用ASCII方式下载文件；

ftpd_banner=Welcome to shuke FTP service：定制欢迎信息；

deny_email_enable=YES：是否禁止匿名用户使用某些邮件地址；

banned_email_file=/etc/vsftpd.banned_emails：如果禁止匿名用户使用某些邮件地址，则输入禁止的邮件地址的路径和文件名；

chroot_list_enable=YES：是否将系统用户限制在自己的home目录下；

chroot_list_file=/etc/vsftpd.chroot_list：如果限制系统用户在home目录下，则在列表中写出被禁止的用户列表；

max_clIEnts=Number：如果以standalone模式启动，那么，只有$Number个用户可以连接，其他用户将得到错误信息，缺省是0，不限制用户数；

message_file：设置访问一个目录时获得的目录信息文件的文件名，缺省是.message。

请注意在配置文件中缺省的设置主要是出于安全考虑。上面每一个改变都会使系统的安全性更小，所以请只在您需要时才改变他们。

</div>