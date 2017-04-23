---
title: 软件搬家工具，自己丰衣足食！
tags:
  - 360软件搬家
  - c盘搬家软件
  - 手机软件搬家工具
  - 电脑软件搬家
  - 软件搬家
id: 674
categories:
  - 软件包
date: 2012-12-08 22:59:05
---

[caption id="attachment_679" align="aligncenter" width="602"][![](http://www.pooy.net/wp-content/uploads/2012/12/junction.jpg "junction")](http://www.pooy.net/wp-content/uploads/2012/12/junction.jpg) junction[/caption]

我早已不在宿主机使用中国大陆地区装机量极大的各种卫士、管家软件，其之所以能流行，我想其正是切合了用户的需求：简单。有兴趣的话，不妨阅读：《真实的用户，真实的中国互联网》。

我对这类软件的态度，在《集系统优化清理维护于一身的Advanced SystemCare 5》一文表述得再明白不过。引述韩寒在《一些琐事》写的一句话：“我们总说，这个社会需要常识，需要启蒙，但其实我认为，互联网十年，该启蒙的人已经被启蒙了，有常识的人一直有常识”。

如果你正准备把安装在系统盘的软件转移到其他位置而安装卫士、管家之类的软件，不妨用几分钟的时间，看完下面的文字。

&nbsp;

纵观市面上流行的带软件搬家功能的软件，都存在：仅支持NTFS分区这一前提。

这就为我们了解软件搬家原理及其实现方法找到了突破口。为什么这么说？

可以参考：微软TechNet中文IT技术社区Junction的下载介绍（英文）或《NTFS 新特性：Junction 应用详解》。

简而言之，Junction即：本地NTFS磁盘前提下，将真实存在的目录，链接到一个或多个位置，对任一位置的编辑，都对所有位置生效，而占用磁盘空间的仅是真实存在的目录。这类似于快捷方式，却又不同，在Windows看来，Junction的目录链与真实的目录无异。

下面就不再赘述其原理，结合具体例子，谈软件搬家的实现。

&nbsp;

我们假设系统盘符为C，谷歌拼音输入法安装在系统盘。而现在C盘空间不足，用户希望能将谷歌拼音输入法转移到D盘。

我知道有人会问：“为什么不卸载/重新安装”？熟悉Google的用户都知道，包括谷歌拼音输入法在内的大部分的Google软件，是不能选择安装目录的。

原安装目录：C:\Program Files\Google\Google Pinyin 2

转移目标目录：D:\Program Files\Google\Google Pinyin 2

&nbsp;

具体操作：

1.移动原安装目录到D:\Program Files下，即：”D:\Program Files\Google\Google Pinyin 2”；

2.转移目标目录链接到原安装目录，即：”D:\Program Files\Google\Google Pinyin 2”。

&nbsp;

创建目录链：
<pre class="brush: bash; gutter: true">=====================无奈的分割线=====================
junction &quot;C:\Program Files\Google\Google Pinyin 2&quot; &quot;D:\Program Files\Google\Google Pinyin 2&quot;
=====================无奈的分割线=====================</pre>
&nbsp;

&nbsp;

删除目录链：
<pre class="brush: bash; gutter: true">=====================悲悯的分割线=====================
junction -d &quot;C:\Program Files\Google\Google Pinyin 2&quot;</pre>
此文转载于互联网，如有涉及版权问题请及时与璞玉联系！

&nbsp;

[dlw did="1" t="junction下载"][junction下载](http://qd2.pc6.com/qd1/Junction.zip "Junction下载地址")[/dlw]

## 下载完junction之后，拷贝到Windows目录哦。

&nbsp;