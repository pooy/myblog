---
title: 解决Dedecms后台swfupload上传图片302错误（500error）
tags:
  - DEDECMS
  - dedecms上传图片302
  - swfupload 302
  - swfupload报302错误
id: 1740
categories:
  - DEDECMS
  - 开源软件
date: 2014-01-23 23:48:55
---

有朋友给我说之前给他弄的一个[dedecms](http://www.pooy.net/category/os/%E7%BB%87%E6%A2%A6cms "dedecms")的管理后台，图片上传出问题了。出现红色方块的错误，导致图片上传不上去、如下图：

[![dedecms_swfupload](http://www.pooy.net/wp-content/uploads/2014/04/dedecms_swfupload.jpg)](http://www.pooy.net/wp-content/uploads/2014/04/dedecms_swfupload.jpg)

之前还真的没遇到过，调试后发现是上传大小的限制问题。

1，修改php.ini 中的：
<pre class="brush: php; gutter: true">upload_max_filesize=2M　　 //默认上传文件大小，这个就是2M的限制！</pre>
具体改成多少由你自己决定。如果找不到php.ini，那就写一个phpinfo的脚步自己看看吧。如果php是使用的php-fpm来管理的记得重启php-fpm，重启方法是：
<pre class="brush: bash; gutter: true">pgrep php-fpm |xargs sudo kill -USR2</pre>
重启之后会在php.ini中看到upload_max_filesize的实际值为你刚刚所修改的。

2，修改插件的初始化值：

打开后台的templets/album_edit.htm,找到file_size_limit,然后修改实际的值。同时改掉94行：
<pre class="brush: bash; gutter: true">button_text : &#039;&lt;span class=&quot;button&quot;&gt;选择本地图片 &lt;span class=&quot;buttonSmall&quot;&gt;(单图最大为 20 MB，支持多选)&lt;/span&gt;&lt;/span&gt;&#039;,</pre>
效果如下图所示：

[caption id="attachment_1742" align="aligncenter" width="518"][![dedecms_swfupload_ok](http://www.pooy.net/wp-content/uploads/2014/04/dedecms_swfupload_ok.jpg)](http://www.pooy.net/wp-content/uploads/2014/04/dedecms_swfupload_ok.jpg) dedecms_swfupload_ok[/caption]

&nbsp;

3，修改include/userlogin.class.php文件中的第二行session_start();前加上：
<pre class="brush: bash; gutter: true">       if (isset($_POST[&quot;PHPSESSID&quot;])) {

	session_id($_POST[&quot;PHPSESSID&quot;]);

	} else if (isset($_GET[&quot;PHPSESSID&quot;])) {

	session_id($_GET[&quot;PHPSESSID&quot;]);

	}</pre>
&nbsp;

如果出现：**fatal error allowed memory size of 134217728 bytes exhausted tried to allocate 32 bytes**

那么请修改你的配置文件config.php中添加:
<pre class="brush: php; gutter: true">ini_set(&#039;memory_limit&#039;, &#039;256M&#039;);</pre>
就可以解决了。、

以上就是解决[Dedecms](http://www.pooy.net/category/os/%E7%BB%87%E6%A2%A6cms "dedecms")后台swfupload上传图片302错误（500error）的一些思路.