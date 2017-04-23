---
title: 解决nginx下uploadify的401 HTTP ERROR
tags:
  - http basic auth
  - http basic auth php
  - nginx auth
  - nginx basic auth uploadify 401
  - nginx http auth
id: 1759
categories:
  - Uploadify
  - 开源软件
date: 2014-05-01 17:00:08
---

nginx 下面使用uploadify遇到[401 HTTP ERROR  ](http://www.pooy.net/http-uploadify-error-401.html "http uploadify error 401问题的解决方法").问题的详细说明之前在：《[about uploadify http error 401 (Unauthorized)](http://www.pooy.net/about-uploadify-http-error-401-unauthorized.html "about uploadify http error 401 (Unauthorized)")》中已经详细说明。不过之前的环境是Apache，今天出现这个问题的环境变成了<span style="color: #888888;">Nginx</span>。

直接在配置文件中添加如下配置即可：
<pre class="brush: bash; gutter: true">        #过滤该文件下的upload.php文件不需要认证
        location /admin/Tpl/default/Common/js/ {
            auth_basic off;
            index index.html index.htm index.php;
        }</pre>
试试是不是可以了呢？