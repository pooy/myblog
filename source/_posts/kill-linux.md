---
title: kill 某程序的所有进程
tags:
  - kill 进程
  - linux
  - linux kill 进程名
  - linux kill进程
id: 723
categories:
  - Linux
  - shell相关
date: 2012-12-15 23:01:15
---

<div>
<div>如何kill掉进程名包含某个字符串的一批进程:</div>
<pre class="brush: bash; gutter: true">kill -9 $(ps -ef|grep 进程名关键字|awk &#039;$0 !~/grep/ {print $2}&#039; |tr -s &#039;\n&#039; &#039; &#039;)</pre>
<div></div>
<div>观测进程名包含某个字符串的进程详细信息:
<pre class="brush: bash; gutter: true">top -c -p $(ps -ef|grep 进程名关键字|gawk &#039;$0 !~/grep/ {print $2}&#039; |tr -s &#039;\n&#039; &#039;,&#039;|sed &#039;s/,$/\n/&#039;)</pre>
</div>
<div></div>
<div>在ubuntu下面会报错：/bin/sh: gmake: not found</div>
用 type gmake
看看这些命令是否安装了
原因:在ubuntu中已经取消掉了gmake，都用make代替。
原因:在ubuntu中已经取消掉了gawk，都用awk代替。
<div></div>
<div> 解决:
<pre class="brush: bash; gutter: true">$ sudo ln -s /usr/bin/make  /usr/bin/gmake
 $ sudo apt-get install gawk</pre>
或者安装：
<pre class="brush: bash; gutter: true">$ sudo apt-get install gawk</pre>
</div>
</div>