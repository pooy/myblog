---
title: linux系统命令查看系统，主板，显卡，cpu，bios等信息
tags:
  - centos查看主板
  - centos查看显卡型号
  - linux
  - ubuntu
  - ubuntu查看主板
id: 1502
categories:
  - Linux
  - Ubuntu
  - 手册资料
date: 2012-10-28 18:00:42
---

<div>

[璞玉](http://www.pooy.net "璞玉")收集整理了一些linux系统命令查看**系统**，**主板，显卡，cpu，bios**等信息。

**系统**

# uname -a               # 查看内核/操作系统/CPU信息
<div># head -n 1 /etc/issue   # 查看操作系统版本</div>
<div>**
**</div>
<div>**#more /etc/redhat-release  # 查看操作系统版本**</div>
# cat /proc/cpuinfo      # 查看CPU信息

# hostname               # 查看计算机名

# lspci -tv              # 列出所有PCI设备

# lsusb -tv              # 列出所有USB设备

# lsmod                  # 列出加载的内核模块

# env                    # 查看环境变量

&nbsp;

资源

# free -m                # 查看内存使用量和交换区使用量

# df -h                  # 查看各分区使用情况

# du -sh &lt;目录名&gt;        # 查看指定目录的大小

# grep MemTotal /proc/meminfo   # 查看内存总量

# grep MemFree /proc/meminfo    # 查看空闲内存量

# uptime                 # 查看系统运行时间、用户数、负载

# cat /proc/loadavg      # 查看系统负载

&nbsp;

磁盘和分区

# mount | column -t      # 查看挂接的分区状态

# fdisk -l               # 查看所有分区

# swapon -s              # 查看所有交换分区

# hdparm -i /dev/hda     # 查看磁盘参数(仅适用于IDE设备)

# dmesg | grep IDE       # 查看启动时IDE设备检测状况

&nbsp;

网络

# ifconfig               # 查看所有网络接口的属性

# iptables -L            # 查看防火墙设置

# route -n               # 查看路由表

# netstat -lntp          # 查看所有监听端口

# netstat -antp          # 查看所有已经建立的连接

# netstat -s             # 查看网络统计信息

&nbsp;

进程

# ps -ef                 # 查看所有进程

# top                    # 实时显示进程状态

&nbsp;

用户

# w                      # 查看活动用户

# id &lt;用户名&gt;            # 查看指定用户信息

# last                   # 查看用户登录日志

# cut -d: -f1 /etc/passwd   # 查看系统所有用户

# cut -d: -f1 /etc/group    # 查看系统所有组

# crontab -l             # 查看当前用户的计划任务

&nbsp;

服务

# chkconfig --list       # 列出所有系统服务

# chkconfig --list | grep on    # 列出所有启动的系统服务

&nbsp;

程序

# rpm -qa                # 查看所有安装的软件包

&nbsp;

&nbsp;

内存

free [－b　－k　－m] [－o] [－s delay] [－t] [－V]

ｃ.主要参数

－b －k －m：分别以字节（KB、MB）为单位显示内存使用情况。

－s delay：显示每隔多少秒数来显示一次内存使用情况。

－t：显示内存总和列。

－o：不显示缓冲区调节列。

ｄ.应用实例

free命令是用来查看内存使用情况的主要命令。和top命令相比，它的优点是使用简单，并且只占用很少的系统资源。通过－S参数可以使用free命令不间断地监视有多少内存在使用，这样可以把它当作一个方便实时监控器。

＃free －b －s5
<div>使用这个命令后终端会连续不断地报告内存使用情况（以字节为单位），每5秒更新一次。</div>
</div>
<div></div>
<div>#如何查看显卡型号？</div>
<div> lspci |grep VGA</div>
<div></div>
<div>**查看BIOS**</div>
<div>#dmidecode</div>
<div>其实通过查看bios，基本能看清楚服务器的所有硬件的情况。</div>
<div>详情请直接点击：[Linux/centos/ubuntu下查看主板BIOS信息](http://www.pooy.net/view-the-motherboard-bios-information-under-linuxcentosubuntu.html "Linux/centos/ubuntu下查看主板BIOS信息")</div>