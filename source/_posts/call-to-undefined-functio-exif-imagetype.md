---
title: PHP处理图片之一php处理水印
tags:
  - Call to undefined function exif_imagetype()
  - php图片类
  - php处理水印
id: 61
categories:
  - PHP
  - 文件处理
date: 2011-08-16 18:29:57
---

关键字：php处理水印,Call to undefined function exif_imagetype(),php图片类

今天在写一个图片在线系统的时候，遇到了几个棘手的问题！如下：

处理水印时,遇到了 Call to undefined function exif_imagetype()错误！

在网上找了个解决方法:

必须在php.ini里面打开<span>[ccei]extension=php_mbstring.dll[/ccei]</span> 和<span>[ccei] extension=php_exif.dll[/ccei]</span>
并保证, <span>[ccei]extension=php_mbstring.dll[/ccei]</span>在 <span>[ccei]extension=php_exif.dll[/ccei]</span>之前
<pre>[cce]
Table 1\. Imagetype Constants
Value         Constant
1         IMAGETYPE_GIF
2         IMAGETYPE_JPEG
3         IMAGETYPE_PNG
4         IMAGETYPE_SWF
5         IMAGETYPE_PSD
6         IMAGETYPE_BMP
7         IMAGETYPE_TIFF_II (intel byte order)
8         IMAGETYPE_TIFF_MM (motorola byte order)
9         IMAGETYPE_JPC
10         IMAGETYPE_JP2
11         IMAGETYPE_JPX
12         IMAGETYPE_JB2
13         IMAGETYPE_SWC
14         IMAGETYPE_IFF
15         IMAGETYPE_WBMP
16         IMAGETYPE_XBM
[/cce]</pre>
[![](http://www.pooy.net/wp-content/uploads/2012/07/1253679991vbyFzJel.png "PHP图片处理")](http://www.pooy.net/wp-content/uploads/2012/07/1253679991vbyFzJel.png)

&nbsp;

[PHP处理图片之二报A non well formed numeric value encountered错误处理](http://www.pooy.net/?p=63)

&nbsp;