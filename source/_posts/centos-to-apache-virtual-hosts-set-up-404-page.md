---
title: Centos给apache虚拟主机设置404页面
tags:
  - apache虚拟主机设置404页面
  - centos虚拟主机404页面设置
  - centos虚拟主机配置
  - IIS设置404页面
id: 1036
categories:
  - Linux
  - 手册资料
date: 2012-04-03 17:25:16
---

新装的一台VPS没有设置404页面，确实有点不太友好。所有今天在这里我主要说明下centos设置apache虚拟主机404页面（Linux服务器），以及IIS服务器设置404页面的技巧方法。

**一、 Apache下设置404页面（一般是Linux主机）**
为Apache Server设置 404错误页面的方法很简单，只需：
(1)在.htaccess 文件中加入如下内容：ErrorDocument 404 /404.php，将.htaccess文件上传到网站根目录
(2)制作一个404页面，随便您设计，命名为404.php，同样上传到网站根目录。

这样子就能每个虚拟主机设置不一样的404页面了

**二、 IIS/.net下设置404页面**

在IIs中设置方法：打开IIS管理器–&gt;点击要设置自定义404的网站的属性–&gt;点击自定义错误选项–&gt;选中404页–&gt;选中并打开编辑属性–&gt;设置成 URL –&gt; URL 里填写“/404.html”–&gt;按确定退出再把做好的404.html 页面上传到网站根目录下。此处在“消息类型”中一定要选择“文件”或“默认值”，而不要选择“URL”，不然，将导致返回“200”状态码。

另外一点要注意的是，IIS服务器上面设置404页面，如果是“消息类型”里面选择"URL"，则必须使用ASP文件（因为只有ASP页面才能返回404状态码），否则访问出错404页面就会返回200状态码。Asp文件的顶部一定要添加以下这个代码：
<pre class="brush: powershell; gutter: true"> &lt;%Response.Status = &quot;404 Not Found&quot;%&gt;</pre>