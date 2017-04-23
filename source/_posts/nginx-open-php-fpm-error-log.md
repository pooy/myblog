---
title: 关于Nginx下开启php-fpm 输出php错误日志的设置(已解决)
tags:
  - nginx
  - nginx php-fpm
  - php-fpm error_log
id: 1724
categories:
  - 生活点滴
date: 2014-02-21 17:31:36
---

最近在本地搭建的LNMP的开发环境。为了开发的时候不影响前端的正常开发就屏蔽的PHP里面php.ini中的一些错误提示。但是这样一来，就影响到了后端开发的一些问题比如不能及时调试开发中的一些问题。

nginx与apache不一样，在apache中可以直接指定php的错误日志，那样在php执行中的错误信息就直接输入到php的错误日志中，可以方便查询。

在nginx中事情就变成了这样：nginx只对页面的访问做access记录日志。不会有php的error log 信息。nginx把对php的请求发给php-fpm fastcgi进程来处理，默认的php-fpm只会输出php-fpm的错误信息，在php-fpm的errors log里也看不到php的errorlog。

原因是php-fpm的配置文件php-fpm.conf中默认是关闭worker进程的错误输出，直接把他们重定向到/dev/null,所以我们在nginx的error log 和php-fpm的errorlog都看不到php的错误日志。

所以我们要进行如下的设置就能查看到nginx下php-fpm不记录php错误日志的方法：

1,修改php-fpm.conf中的配置，如果没有请增加:
<pre class="brush: bash; gutter: true">[global]
; Note: the default prefix is /usr/local/php/var
error_log = log/php_error_log 
[www]
catch_workers_output = yes</pre>
<div>

2.修改php.ini中配置，没有则增加
<pre class="brush: bash; gutter: true"> log_errors = On
 error_log = &quot;/usr/local/php/var/log/error_log&quot;
 error_reporting=E_ALL&amp;~E_NOTICE</pre>
3.重启php-fpm，
当PHP执行错误时就能看到错误日志在"/usr/local/lnmp/php/var/log/php_error_log"中了

</div>
<div>如果出现：</div>
<pre class="brush: bash; gutter: true">[root@localhost etc]# service php-fpm restart
Gracefully shutting down php-fpm . done
Starting php-fpm [17-Apr-2014 18:40:52] ERROR: [/usr/local/php/etc/php-fpm.conf:5] unknown entry &#039;catch_workers_
[17-Apr-2014 18:40:52] ERROR: failed to load configuration file &#039;/usr/local/php/etc/php-fpm.conf&#039;
[17-Apr-2014 18:40:52] ERROR: FPM initialization failed
 failed</pre>
那请在第一步的时候，认真将配置写入相对应的组中，不然就出现上面的：**ERROR: [/usr/local/php/etc/php-fpm.conf:5] unknown entry 'catch_workers_output'**

最后看看效果：

[![php-fpm](http://www.pooy.net/wp-content/uploads/2014/04/php-fpm.jpg)](http://www.pooy.net/wp-content/uploads/2014/04/php-fpm.jpg)

&nbsp;

[![error](http://www.pooy.net/wp-content/uploads/2014/04/error.jpg)](http://www.pooy.net/wp-content/uploads/2014/04/error.jpg)