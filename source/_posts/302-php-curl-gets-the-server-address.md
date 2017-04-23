---
title: PHP中CURL获取服务器302地址
tags:
  - CURL
  - CURL 302
  - php CURL 302
id: 1225
categories:
  - CURL
  - PHP
date: 2013-04-09 18:00:44
---

<div>
<div>PHP中[CURL](http://www.pooy.net/category/php-2/curl "curl")获取服务器302地址，这是写一个API的时候，首先是请求那个接口之后，返回来的是一串认证码，然后通过这个认证码再去登陆。</div>
<div></div>
<pre class="brush: php; gutter: true">      /*返回一个302地址*/
     function  curl_post_302($url, $vars) {

          $ch = curl_init();
          curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
          curl_setopt($ch, CURLOPT_URL,  $url);
          curl_setopt($ch, CURLOPT_POST, 1);
          curl_setopt($ch,  CURLOPT_FOLLOWLOCATION, 1); // 302 redirect
          curl_setopt($ch,  CURLOPT_POSTFIELDS, $vars);
          $data = curl_exec($ch);
          $Headers =  curl_getinfo($ch);
          curl_close($ch);
          if ($data != $Headers)
          return  $Headers[&quot;url&quot;];
          else
          return false;

     }</pre>
<div></div>
<div>上面的这个curl_post_302 函数就可以直接取到302跳转地址了.</div>
</div>