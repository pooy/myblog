---
title: jquery.uploadify动态传递表单元素
tags:
  - jquery.uploadify传参
  - jquery.uploadify动态传递表单参数详解
  - uploadify
id: 234
categories:
  - Uploadify
  - 开源软件
date: 2012-08-15 17:55:49
---

jquery.[uploadify](http://www.pooy.net/category/network-programming/uploadify "uploadify")动态传递表单元素

在给网站开发的时候，[璞玉](http://www.pooy.net "璞玉")需要用到uploadify这个上传插件，在使用的时候，遇到一个问题就是通过前端的上传脚本，把一个动态的数据传递到上传后台处理页面做一个参数。

看了手册之后发现有一个接口，可以使用。那就是'scriptData',（这个是在Uploadify3.0的版本下才有，3.0以上改为formData）.

在使用uploadify时，如果使用初始化参数的方式传递参数，会发现修改过的表单元素传不到后台。
<pre class="brush: javascript; gutter: true">&#039;scriptData&#039;    : {&#039;ttype&#039;:document.getElementById(&#039;name&#039;).value},</pre>
仔细分析了一下，这里传递的参数是表单初始化的时候值，所以一定是空的，或者是默认的。

解决方法是在提交表单时，加上这么一句代码：
<pre class="brush: php; gutter: true">&lt;a href=&quot;javascript:$(&#039;#uploadify&#039;).uploadifySettings(&#039;scriptData&#039;,{&#039;ttype&#039;:document.getElementById(&#039;name&#039;).value}); jQuery(&#039;#uploadify&#039;).uploadifyUpload()&quot;&gt;开始上传&lt;/a&gt;</pre>
<span>
</span>

注意书写格式，不然会出错的。

如果对[uploadify ](http://www.pooy.net/tag/uploadify-2 "uploadify")参数不是很了解的话，可以参考之前写的那篇[《Uploadify与php使用详解](http://www.pooy.net/uploadify-php.html "Uploadify与php使用详解")》 ，里面每个参数都有注解。希望对大家有帮助！有问题可以直接留言。

#### [uploadify 参数列表（已翻译）](http://www.pooy.net/uploadify-parameters.html "链接到 uploadify 参数列表（已翻译）")

#### [uploadify3.1 PHP+formData动态传值 Dome下载](http://www.pooy.net/dynamic-dome-uploadify3-1-phpformdate-download.html "链接到 uploadify3.1 PHP+formData动态传值 Dome下载")