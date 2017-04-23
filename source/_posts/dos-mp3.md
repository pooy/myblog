---
title: dos 查找所有MP3文件复制到另一个目录
tags:
  - dos 查找MP3格式文件遍历复制到另一个目录
  - dos查找文件
  - dos遍历复制文件
id: 246
categories:
  - Windows
date: 2012-08-16 16:15:27
---

今天折腾了下Ipod，升级了itunes到最新版，准备把之前千千静听里面下载的歌曲导入到ipod里面的时候出现了一个小问题，就是千千静听默认的是以一个个歌手名字命名的文件夹的形式来保存已下载的歌曲。

这就有点小头疼了。要是在linux里面还好说，写个shell脚本就行了。这个是在windows里面、可以手动一个个复制，都是要是几百个文件夹呢？几千个呢？那岂不是要ctrl+c &amp;ctrl+v 直到手麻？

于是就在网上查了下dos 命令，写了个bat脚本！
<pre class="brush: bash; gutter: true">@echo off
echo 正在复制，请稍候...
for /f &quot;delims==&quot; %%a in (&#039;dir /b /s e:\TTPmusic\*.mp3&#039;)do copy /-y &quot;%%a&quot; e:\new
::虽说是新方法，但for命令还是少不了的，不过这次是用的/f参数使其分析dir命令的输出结果，并用dir 的/s参数搜寻子目录来达到效果的。
echo 复制完成，祝你愉快\(^o^)/~
pause</pre>
使用方法：1，复制之后另存为一个.bat的文件，双击执行即可！/-y 是不提示！ 2，NEW这个文件夹一定要存在！

[caption id="attachment_253" align="aligncenter" width="302"][![dos](http://www.pooy.net/wp-content/uploads/2012/08/dos01.jpg "dos01")](http://www.pooy.net/wp-content/uploads/2012/08/dos01.jpg) dos[/caption]
<pre> &lt;a title=&quot;璞玉&quot; href=&quot;http://www.pooy.net&quot; target=&quot;_blank&quot;&gt;璞玉&lt;/a&gt;的音乐文件夹在E盘的TTPmusic大文件下面，上面的意思是把这个文件夹下面的所以mp3都复制到E盘下面的new文件夹里面。
这不就OK了么？</pre>