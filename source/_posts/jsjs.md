---
title: JS关闭窗口不提示或JS关闭页面的几种方法
tags:
  - JS关闭窗口不提示
  - JS关闭页面的几种方法
id: 43
categories:
  - Javascript
date: 2012-02-13 11:06:48
---

之前开发的时候，没有考虑编辑们工作量的问题，他们审核之后，[璞玉](http://www.pooy.net "璞玉")(pooy)直接用window.close()来写的，弹出一个对话框，提示是否关闭。随着他们的工作量越来越大了，这个弹出框反而成了累赘。所以写了几个小测试的脚步。
第一种：点击链接没有提示的JS关闭窗口
<pre class="brush: html; gutter: false">&lt;a href=&quot;javascript:window.close()&quot; &gt;关闭窗口&lt;/a&gt;</pre>
<span>
</span>

&nbsp;

第二种：JS定时自动关闭窗口
<pre class="brush: javascript; gutter: false">&lt;script language=&quot;javascript&quot;&gt;
 &lt;!--
 function closewin()
 {
 self.opener=null;
 self.close();
 }
 function clock()
 {
 i=i-1
 document.title=&quot;本窗口将在&quot;+i+&quot;秒后自动关闭!&quot;;
 if(i&gt;0)setTimeout(&quot;clock();&quot;,1000);
 else closewin();
 }
 var i=10
 clock();
 //--&gt;
 &lt;/script&gt;</pre>
&nbsp;

&nbsp;

第三种：窗口没有提示自动关闭的js代码
<pre class="brush: javascript; gutter: false">&lt;script language=javascript&gt;
 &lt;!--
   window.opener=null;
    window.open( &quot; &quot;,   &quot;_self&quot;);   //IE7必需的.
    window.close();
 //--&gt;
 &lt;/script&gt;</pre>
最后用的就是第三种，窗口自己关闭掉不提示。