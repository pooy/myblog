---
title: axel 断点续传是你下载好帮手
tags:
  - axel
  - axel下载工具
  - axel使用
  - axel断点续传
id: 727
categories:
  - FTP下载周边
  - Linux
date: 2012-12-17 23:10:49
---

axel 断点续传是你下载好帮手！再改用axel做下载之后确实优化了不少！下面来介绍一下axel使用！

之前在做下载的时候，起初是直接用php的只带的ftp_ge()函数来下载，时间一长，问题就出来了。首先是下载的速度得不到保障，如果服务器上某些程序占了相当大的带宽。那这个函数就不太起作用了！

后来就开始使用wget 来做下载。这个在速度上确实要比ftp_ge()函数有保障！不过要是遇到网络问题，出现断网的情况，那就不痛快了。

万一一个软件下载了一半，网断掉了，那么网恢复的时候，wget就会默认这个软件已经下载好了，因为在去下载一个文件之前，他会先生成一个.list文件，来做记录！如果直接去打开这个问题，那就是文件损坏。

axel这个工具确实能解决上述的两个问题，一个是网络问题，另一个是断线续传！

axel这个还是一个多进程下载工具！

axel安装只要一条命令即可！在ubuntu下面直接输入：
<pre class="brush: bash; gutter: true">apt-get install axel</pre>
下面介绍一下axel的参数：

使用简介

大家首先可以用命令

axel -h查看axel使用方法。

基本的用法如下：
<pre class="brush: bash; gutter: true">　　-o [f]：指定本地输出文件。
　　-S [x]：搜索镜像并从X servers服务器下载。
　　-N：不使用代理服务器。
　　-v：打印更多状态信息。
　　-a：打印进度信息。
　　-h：该版本命令帮助。
　　-V：查看版本信息号。
　　-s [x]：指定每秒下载最大比特数。
　　n [x]：指定同时打开的线程数。</pre>
&nbsp;

实例：
<pre class="brush: bash; gutter: true"> 　axel -n 10 -a http://www.pooy.net/uploadify.rar</pre>
上面这个实例是下载[uploadify](http://www.pooy.net/uploadify-php.html "uploadify 教程")这个压缩包，开十个进程去下载，保存在当前目录，如果要改变直接添加-o参数指定即可！

这就是一个多进程，而且还支持断点续传的工具，他就是axel！