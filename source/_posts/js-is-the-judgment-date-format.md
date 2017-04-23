---
title: js正则判断日期格式
tags:
  - js正则判断日期格式
  - 判断输入框中输入的日期格式为yyyy-mm-dd和正确的日期
id: 1004
categories:
  - Javascript
date: 2013-01-25 22:43:52
---

<pre class="brush: javascript; gutter: true">/**  
     判断输入框中输入的日期格式为yyyy-mm-dd和正确的日期  
   */  
  function   IsDate(sm,mystring)   {  
      var   reg   =   /^(\d{4})-(\d{2})-(\d{2})$/;  
      var   str   =   mystring;  
      var   arr   =   reg.exec(str);  
      if   (str==&quot;&quot;)   return   true;  
      if   (!reg.test(str)&amp;&amp;RegExp.$2&lt;=12&amp;&amp;RegExp.$3&lt;=31){  
        alert(&quot;请保证&quot;+sm+&quot;中输入的日期格式为yyyy-mm-dd或正确的日期!&quot;);  
        return   false;  
        }  
        return   true;  
    }</pre>
&nbsp;