---
title: 'bash: /usr/bin/scp: Input/output error 解决方法'
tags:
  - Input/output error
  - linux
  - SFTP subsystem request is rejected
id: 664
categories:
  - Linux
  - linux错误锦集
date: 2012-12-05 23:22:30
---

<div> 登陆服务器准备拷贝数据：
<div>-bash: /usr/bin/scp: Input/output error</div>
<div>发现如上错误，网上查询，说是硬盘挂掉了、</div>
<div>但是为什么别的网站引用的这个硬盘上的数据为什么还是可读的呢？</div>
<div>
<div>于是打开ftp报错:</div>
<div>SFTP subsystem request is rejected .Please make sure that SFTP subsystem is  properly installed in SSH server</div>
</div>
<div></div>
<div>网上说用： e2fsck 之后，可以正常拷贝。不过还是三思而后行之！唯恐数据不在。</div>
</div>
<div></div>
<div></div>