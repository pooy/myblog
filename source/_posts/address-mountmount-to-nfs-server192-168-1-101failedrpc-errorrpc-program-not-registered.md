---
title: '解决mount: mount to NFS server ''192.168.1.101'' failed: RPC Error:RPC：程序未注册..'
tags:
  - linux
  - mount
  - mount to NFS server
id: 1207
categories:
  - Linux
  - linux错误锦集
date: 2013-04-07 23:22:18
---

#### 解决mount: mount to NFS server '192.168.1.101' failed: RPC Error:RPC：程序未注册..

<pre class="brush: bash; gutter: false">[root@localhost etc]# mount -t nfs localhost:/opt/photoframe/photos/ /home/meng/testshare/</pre>
**
出现下面的错误
mount: mount to NFS server 'localhost' failed: RPC Error: Program not registered.
后来在网上发现下面说的重启nfs服务，另外修改了exports文件后，要exportfs -rv使之生效
**服务器IP：172.0.0.1，主机名：p470-1, 通过NFS共享/disk1目录

在客户端使用 mount -t nfs p470-1:/disk1 /disk1
时出现"mount: mount to NFS server 'p470-1' failed: RPC Error: Program not registered."错误提示。
出错原因：p470-1由于网络原因nfs服务被中断，重新开启p470-1的nfs服务然后在客户端重新mount disk1即可
service nfs restart 或 /etc/rc.d/init.d/nfs restart

**注:客户端与服务端都安装portmap和nfs并启动再mount**

《[解决mount.nfs: access denied by server while mounting](http://www.pooy.net/address-mount-nfsaccess-denied-by-server-while-mounting.html "链接到 解决mount.nfs: access denied by server while mounting")》