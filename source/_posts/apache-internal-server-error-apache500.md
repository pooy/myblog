---
title: apache Internal Server Error Apache详细解决500错误
tags:
  - 500错误
  - apache Internal Server Error
  - 详细解决500错误
id: 41
categories:
  - Linux
  - linux错误锦集
date: 2012-04-12 18:05:46
---

今天要把一个服务器升级，系统重装。

必须得先把之前的程序copy到另外的机子上去跑。

第一步，copy文件

第二步，查看另外一台服务器的配置文件，然后找到指定的document。

查看Apache 配置文件:首先 updatedb  然后执行locate httpd.conf

找到httpd.conf之后，查看虚拟目录

# Internal Server Error

The server encountered an internal error or misconfiguration and was unable to complete your request.

Please contact the server administrator, webmaster@dummy-host2.pooy.net and inform them of the time the error occurred, and anything you might have done that may have caused the error.

More information about this error may be available in the server error log.

&nbsp;
<pre>根据经验判断，500错误基本都是服务器的错误，立即去检测apache的配置文件，以及文件的权限问题！
最后发现了一个文件：.htaccess,顿时恍然大悟！立即删之，即可！</pre>
&nbsp;

&nbsp;