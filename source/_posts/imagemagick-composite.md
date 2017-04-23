---
title: 使用imageMagick中composite命令帮助你合并图片
tags:
  - composite
  - imagemagick
  - imageMagick composite合并图片
  - imagemagick命令
id: 734
categories:
  - Imagemagick
  - 开源软件
date: 2012-12-19 15:03:31
---

<div>一般的网站，在首页面都会涉及轮播滚动的DIV，不管是用flash写也好，或者用[jquery](http://www.pooy.net/tag/jquery-uploadify "jquery-uploadify")插件写也罢。最终还是需要规格一致的图片。
<div>毕竟图片才是内涵！</div>
<div>最近遇到一个小问题，那个轮播的DIV的长宽是固定的，长宽分别为：244x220。而图片是不能确定的。有的是长图有的是横图。如果直接发到div里面去显示，有可能就遇到图片拉伸 了，那样子很影响图片的美观。</div>
<div>这里如果用单纯的convert来操作可能就有点力不足心了。这里要具体介绍一下composite。</div>
<div>composite是 [imagemagick](http://www.pooy.net/imagemagick-chinese-manual.html "ImageMagick 中文手册 ")中用来合并图片的。实例如下:</div>
<div>
<pre class="brush: php; gutter: true">//将长图补为方图
 $cmd=PHOTOCOME_IMAGEMAGICKDIR.&quot;composite -gravity Center -compose in $tmpfile $square $tmpfile&quot;;
 system($cmd);
 echo $cmd.&quot;&lt;/br&gt;&quot;;</pre>
</div>
<div></div>
<div>在$cmd中有三个参数，分别为：
<pre class="brush: php; gutter: true">$tmpfile $square $tmpfile</pre>
</div>
<div></div>
<div>第一个是为前景图片地址</div>
第二个是为背景图片。
<div>第三个是为叠加后的结果</div>
<div></div>
<div>前景图就是要展现的图片，背景图就是来补图的图片，一般为白色或者灰色，主要看网站的色调了。最后把叠加后的结果输入给页面显示就OK了。</div>
<div>

-gravity north 指叠加位置为垂直据顶部、水平居中（正北方向）

如果要求在正中间，参数为center

如果要求在右下角，参数为southeast

更多imagemagick参数，可以查看[ImageMagick 中文手册 ](http://www.pooy.net/imagemagick-chinese-manual.html "ImageMagick 中文手册")！

</div>
</div>