---
title: about uploadify http error 401 (Unauthorized)
tags:
  - http error 401，Unauthorized
  - http uploadify error 401
  - uploadify
  - uploadify http error
  - uploadify http error 401 (Unauthorized)
id: 1291
categories:
  - PHP
  - Uploadify
  - 开源软件
  - 文件处理
date: 2013-04-30 13:12:08
---

<div>

上次说过一个 《[http uploadify error 401问题的解决方法](http://www.pooy.net/http-uploadify-error-401.html "链接到 http uploadify error 401问题的解决方法")》的方法，使用[uploadify 3.1](http://www.pooy.net/dynamic-dome-uploadify3-1-phpformdate-download.html "uploadify3.1 PHP+formData动态传值 Dome下载")的版本,然后添加session验证！但是不是每次都奏效。这次遇到的这个问题有点奇怪，就是在火狐下始终是401，尽管我尝试了很多版本，还是没有解决。
<div></div>
<div>然后仔细想了下，**401就是权限未认证（Unauthorized）**。也就是说是服务器的权限没有认证。我使用的服务器是apache，我检查了下配置文件：</div>
<div></div>
<div>
<pre class="brush: bash; gutter: false">   AllowOverride AuthConfig
    Order Allow,Deny</pre>
</div>
<div></div>
<div>
<div>[AllowOverride AuthConfig](http://www.pooy.net/apache-allowoverride-authconfig.html "apache-allowoverride-authconfig") 这个就是说开启用户验证功能，也就是说访问者要想浏览这个地址必须输入apache服务器的用户验证，然后才能接下来做一些业务处理。</div>
</div>
<div>

[caption id="attachment_1292" align="aligncenter" width="766"][![](http://www.pooy.net/wp-content/uploads/2013/05/basic-Unauthorized.jpg "basic Unauthorized")](http://www.pooy.net/wp-content/uploads/2013/05/basic-Unauthorized.jpg) basic Unauthorized[/caption]

</div>
<div></div>
<div>uploadify是使用的flash进行多选，然后通过ajax异步上传。ajax往服务器上传数据的时候没有进行验证，所以才会出现401的问题。</div>
<div></div>
<div>然后查看了一下根目录的.htaccess文件：</div>
<div></div>
<pre class="brush: bash; gutter: false">AuthName &quot;login&quot;
AuthType Basic 
AuthUserFile /www/.htpasswd
require user admin</pre>
<div></div>
<div>**<span style="color: #ff0000;">上面这个配置是只允许用户为admin的用户认证。</span>**</div>
<div></div>
<div>介于我的上传组件是单独一个文件夹，所以我想在basic验证的时候能不能不去验证这个子目录？</div>
<div></div>
<div>于是在apache的配置文件httpd.conf添加这个uploadify文件夹不需要验证了：</div>
<div>
<pre class="brush: bash; gutter: false">    &lt;Directory &quot;/www/uploadify&quot;&gt;
         Allow from all
         Satisfy any
     &lt;/Directory&gt;</pre>
</div>
<div>或者添加那个处理的uploadify.php文件不需要验证：</div>
<div>
<pre class="brush: bash; gutter: false">&lt;FilesMatch &quot;/www/uploadify/uploadify.php&quot;&gt;
Allow from all
Satisfy any
&lt;/FilesMatch&gt;</pre>
</div>
<div>然后再去使用[uploadify](http://www.pooy.net/?s=uploadify "uploadify")上传，就解决了。IE,foxfire，safari，chrome，opera都能正常上传了。</div>
</div>
<div></div>

#### <span style="text-decoration: underline;">如果是Nginx用户，请移步</span>：《[解决nginx下uploadify的401 HTTP ERROR](http://www.pooy.net/nginx-uploadify-401-http-error.html "解决nginx下uploadify的401 HTTP ERROR")》

#### **推荐阅读：《**<span style="color: #ff0000;">[<span style="color: #ff0000;">**如何添加**Apache服务器用户验证</span>](http://www.pooy.net/apache-allowoverride-authconfig.html "如何添加Apache服务器用户验证")</span>》

<div></div>
<div></div>