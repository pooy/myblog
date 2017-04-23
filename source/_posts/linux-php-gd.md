---
title: 如何给linux服务器安装GD扩展库
tags:
  - linux 安装gd
  - linux安装phpGD扩展
  - linux系统如何让php使用GD库函数
  - redhat安装gd
id: 1194
categories:
  - Linux
  - PHP
  - Ubuntu
  - 手册资料
  - 验证码
date: 2013-03-28 17:39:06
---

<div>

下面说说如何给linux服务器安装GD扩展库，可以分为直接安装，跟编译安装，直接安装一遍用在centos跟ubuntu服务器上，相对而言那些比较老的系统可以只能手动安装了。

本文关键字：linux 安装gd,linux安装phpGD扩展,linux系统如何让php使用GD库函数,redhat安装gd

### 直接安装：

<div>
<div>**centos安装** :</div>
<div>
<pre class="brush: bash; gutter: false">rpm -qa |grep php-gd</pre>
</div>
<div> 如果不存在那么就执行下面的：</div>
<div>
<pre class="brush: bash; gutter: false">yum install php-gd</pre>
</div>
<div>最后重启apache
<pre class="brush: bash; gutter: false">service httpd restart</pre>
</div>
<div>**ubuntu安装:**</div>
<div>
<pre class="brush: bash; gutter: false">apt-get install php-dg</pre>
</div>
</div>
windows安装：

<span> 找到_php_.ini,打开内容,找到: ;extension=_php___gd_2.dll 把最前面的</span>';'分号去掉即可！

### 编译安装：

<div> 编译安装适合那些老版本的红帽系统，大家对GD库应该有所了解，它是PHP进行图文操作时一个重要的库。那么今天我们就来具体讲解一下如何进行php5安装GD库的详细步骤，希望对有需要的朋友有所帮助。</div>
<div>
<div></div>
<div>软件分别为：**zlib-1.2.7.tar，libpng-1.2.40.tar，jpeg-6b.tar，freetype-2.3.5.tar，gd-2.0.33.tar**</div>
<div></div>
打包下载地址：[http://pan.baidu.com/share/link?shareid=452677&amp;uk=3240790330](http://pan.baidu.com/share/link?shareid=452677&amp;uk=3240790330 "linux服务器安装GD扩展库zlib-1.2.7.tar，libpng-1.2.40.tar，jpeg-6b.tar，freetype-2.3.5.tar，gd-2.0.33.tar")

</div>
<div></div>
<div>**PHP5安装GD库1.下载libpng库，至少需要支持一种文件类型，如果需要其他的类型则另外下载**http://nchc.dl.sourceforge.net/project/libpng/00-libpng-stable/1.2.40/libpng-1.2.40.tar.gz解压缩后，进入文件夹
<pre class="brush: bash; gutter: false">cd libpng-1.2.40</pre>
<pre class="brush: bash; gutter: false">mv scripts/makefile.linux ./Makefile #这里一定要使用script下的Makefile ，不要通过./configure</pre>
生成
<pre class="brush: bash; gutter: false">make</pre>
<pre class="brush: bash; gutter: false">make install</pre>
**PHP5安装GD库2.下载freetype库，很多GD函数都需要这个库的支持**

http://ftp.twaren.net/Unix/NonGNU/freetype/freetype-2.3.5.tar.gz

**3.解压后**
<pre class="brush: bash; gutter: false">cd freetype-2.3.5
./configure --prefix=/usr/local/freetype #这里指定freetype的安装目录，以便php编译时用到
make
make install</pre>
**PHP5安装GD库4.下载GD库**

从http://www.libgd.org上选择合适的版本下载

解压后进入目录
<pre class="brush: bash; gutter: false">./configure --prefix=/usr/local/gd2 --with-png --with-freetype #这里不需要制定freetype目录，</pre>
但是需要制定gd库的安装路径
<pre class="brush: bash; gutter: false">make
make install</pre>
**PHP5安装GD库5.编译PHP**

进入php源码目录
<pre class="brush: bash; gutter: false">./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysql --without-sqlite --
without-pdo-sqlite --with-gd=/usr/local/gd2 --with-freetype-dir=/usr/local/freetype/
make
make install</pre>
<div>重启apache即可完成PHP5安装GD库。</div>
</div>
<div></div>
在安装那些包的时候，可能会遇到很大的问题，就是有些包不能下载，璞玉找了很久，终于找到了（其中有一个是翻墙才下下来）。

</div>
<div></div>
<div>软件分别为：zlib-1.2.7.tar，libpng-1.2.40.tar，jpeg-6b.tar，freetype-2.3.5.tar，gd-2.0.33.tar</div>
<div></div>
<div>打包下载地址：[http://pan.baidu.com/share/link?shareid=452677&amp;uk=3240790330](http://pan.baidu.com/share/link?shareid=452677&amp;uk=3240790330 "linux服务器安装GD扩展库zlib-1.2.7.tar，libpng-1.2.40.tar，jpeg-6b.tar，freetype-2.3.5.tar，gd-2.0.33.tar")</div>
<div></div>
<div>解压密码：<span style="color: #ff0000;">**d5475sf45ffdf54**</span></div>
<div></div>
<div>软件整理，来自璞玉POOY,为了保证软件的安全性何可用性。所以都会加上密码。一般密码都会在文章里面会提示出来:http://www.pooy.net/linux-php-gd.html如果有问题，可以随时与[璞玉](http://www.pooy.net "璞玉")交流：www.pooy.net

</div>