---
title: 解决php中使用json_decode显示NULL
tags:
  - php json encode
  - php 解析json
  - php 输出json
  - php 返回json
  - php中使用json_decode 显示NULL
id: 1239
categories:
  - JSON
date: 2013-03-30 22:59:02
---

php中使用json_decode 显示NULL，的原因就是因为json_decode的数据不是严格意义上的UTF-8的编码。

所以需要手动修改转码即可！

使用php的file_get_contents获取API的json数据，在json_decode前使用：
<pre class="brush: php; gutter: true">$json = iconv(&#039;GBK&#039;,&#039;utf-8&#039;, $json);</pre>
转码，然后再使用json_decode(来转码)：
<pre class="brush: php; gutter: true">$new_Arr = json_decode($json, true);</pre>
最后使用var_dump打印出来看看，是不是有数据了？

如果php解析的json数据中文乱码可以查看：