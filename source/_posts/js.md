---
title: JS判断浏览器，来处理浏览器兼容
tags:
  - javascript
  - js判断浏览器
  - 浏览器兼容
id: 227
categories:
  - Javascript
date: 2012-08-14 22:15:59
---

有时候，经常会遇到网页在各个浏览器下不兼容的情况，通常我们处理的方式就是，判断浏览器，然后分别来写不同的JS或者CSS样式，下面就是一些常用的浏览器判断js：
<pre class="brush: javascript; gutter: true">&lt;script&gt;
// 获取浏览器名称及版本信息
 function appInfo(){
 var browser = {
 msie: false, firefox: false, opera: false, safari: false,
 chrome: false, netscape: false, appname: &#039;unknown&#039;, version: 0
 },
 userAgent = window.navigator.userAgent.toLowerCase();
 if ( /(msie|firefox|opera|chrome|netscape)\D+(\d[\d.]*)/.test( userAgent ) ){
 browser[RegExp.$1] = true;
 browser.appname = RegExp.$1;
 browser.version = RegExp.$2;
 } else if ( /version\D+(\d[\d.]*).*safari/.test( userAgent ) ){ // safari
 browser.safari = true;
 browser.appname = &#039;safari&#039;;
 browser.version = RegExp.$2;
 }
 return browser;
 }
 // 调用示例
 var myos = appInfo();
 // 如果当前浏览器是IE，弹出浏览器版本,否则弹出当前浏览器名称和版本
 if ( myos.msie ){
 alert( myos.version );
 } else {
 alert( myos.appname + myos.version );
 }
&lt;/script&gt;</pre>
有了这些判断，做浏览器兼容是不是更容易一些了呢？