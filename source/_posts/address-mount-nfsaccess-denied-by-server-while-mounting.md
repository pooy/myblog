---
title: '解决mount.nfs: access denied by server while mounting'
tags:
  - linux mount
  - linux挂载问题
  - 'mount.nfs: access denied by server while mounting'
id: 1205
categories:
  - Linux
  - linux错误锦集
date: 2013-04-03 14:34:31
---

在linux下挂载的时候突然出现： [mount.nfs: access denied by server while mounting](http://blog.chinaunix.net/uid-20554957-id-3444786.html) 。第一感觉是读取文件权限不够。

然后准备去更改一下挂载点的权限，一看不对。因为其他服务器都能正常挂载那就是说明权限是正确的。

然后问了下哥们，哥们说要添加一条记录到被挂载的服务器上。
修改配置文件/etc/exports，加入 insecure 选项：
<pre class="brush: bash; gutter: true">vi /etc/exports</pre>
添加一条：
<pre class="brush: bash; gutter: false">/bank 255.255.255.255(rw,no_root_squash)</pre>
/bank是挂载点，255.255.255.255是要挂载的IP。
<div>保存退出</div>
<div></div>
<div>然后重启nfs服务：service nfs restart</div>
<div></div>
<div>然后问题就解决了</div>