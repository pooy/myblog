---
title: http uploadify error 401问题的解决方法
tags:
  - http error 401，server error 401，uploadify
  - http uploadify error 401
  - uploadify http error
id: 1178
categories:
  - PHP
  - Uploadify
  - 开源软件
  - 文件处理
date: 2013-03-23 16:56:12
---

去年年中，有幸认识uploadify，并且解决了公司系统前台上传的一些问题，最近有人提出，说要把后台也上传也稍稍改改，改成这个。（目前用的是最原始的file表单提条。虽然可以用ajax随时添加file表单以及稳定，但是速度上太慢了，无法接受！）

实施的时候，又回头看了下去年写的《[Uploadify与php使用详解](http://www.pooy.net/uploadify-php.html "链接到 Uploadify与php使用详解")》，然后就替换到了后台系统框架里面。一切进展的很顺利，然后测试了一张图片，发现没有上传成功：

[caption id="attachment_1180" align="aligncenter" width="475"][![](http://www.pooy.net/wp-content/uploads/2013/03/uploadify-http-error-401.jpg "uploadify http error 401")](http://www.pooy.net/wp-content/uploads/2013/03/uploadify-http-error-401.jpg) uploadify http error 401[/caption]

扎眼一看，HTTP Error错误，查看fireBug，里面的网络情况，发现没有。然后在`onComplete`回调函数后面添加了一个显示错误的函数：
<pre class="brush: javascript; gutter: false">        onError: function(errorObj) {
            alert(errorObj.info+&quot;            &quot;+errorObj.type);
        }</pre>
最后就好使了，错误就能弹出来：

[caption id="attachment_1179" align="aligncenter" width="241"][![](http://www.pooy.net/wp-content/uploads/2013/03/uploadify-http-401.jpg "uploadify http 401")](http://www.pooy.net/wp-content/uploads/2013/03/uploadify-http-401.jpg) uploadify http 401[/caption]

http error 401 这个还不太熟悉，因为常见的是：404,500,302,200这些，所以一时间拿不准，谷歌一下之后才知道这个是权限的问题。

回头检查了下框架的控制器权限，然后重新登录再上传就OK了：

[caption id="attachment_1181" align="aligncenter" width="438"][![](http://www.pooy.net/wp-content/uploads/2013/03/解决-uploadify-http-error-401-.jpg "解决 uploadify http error 401")](http://www.pooy.net/wp-content/uploads/2013/03/解决-uploadify-http-error-401-.jpg) 解决 uploadify http error 401[/caption]

如果在火狐下还是不行的话，升级一下吧.uploadify3.1 及以上的版本不会出现该问题！

如果还是没有解决请查看：

<span style="color: #ff0000;">Nginx：</span>

《[解决nginx下uploadify的401 HTTP ERROR](http://www.pooy.net/nginx-uploadify-401-http-error.html "解决nginx下uploadify的401 HTTP ERROR")》

<span style="color: #ff0000;">Apache:</span>

《[about uploadify http error 401 (Unauthorized)](http://www.pooy.net/about-uploadify-http-error-401-unauthorized.html "about uploadify http error 401 (Unauthorized)")》

&nbsp;

uploadify3.1 PHP+formData动态传值 Dome下载：[http://www.pooy.net/dynamic-dome-uploadify3-1-phpformdate-download.html](http://www.pooy.net/dynamic-dome-uploadify3-1-phpformdate-download.html "uploadify3.1 PHP+formData动态传值 Dome下载")

在实际的开发过程中，可能我们的系统框架都是不一样的，所以需要大伙认真检查了！最后一点就是上传保存的文件夹权限设置为最大。http uploadify error 401问题的解决方法其实就是权限的问题。

## [uploadify 后台处理](http://www.pooy.net/uploadify-houtai.html "链接到 uploadify 后台处理") 推荐看看！