---
title: DEDECMS文章表结构
tags:
  - DEDECMS5.7数据库下载
  - DEDECMS数据库下载
  - DEDECMS文章表结构
  - DEDECMS表结构下载
id: 402
categories:
  - DEDECMS
  - 开源软件
date: 2012-10-28 16:57:35
---

说来话长，[璞玉](http://www.pooy.net/ "璞玉")简短说下。之前用息壤的空间，数据库自动备份的时候全部是乱码。给一个朋友做的站，因为息壤的那台服务器出问题了，需要换台主机。数据是他们技术给我备份的，后来也是他们给我还原的。

后来发现全部是乱码，我就傻眼了。问他们技术，他们推卸责任说是我网站本来就是乱码。我了个去，要是我网站是乱码，网站还能运行大半年了呢？

还好我全站做静态化了，大不了自己再把数据采集一遍。重新入数据。

PS：网站数据我自己备份的是八月中旬的，他们给我的坏数据是九月初的。也就是我只要把这期间的数据重新梳理一遍就没什么大问题了。

于是就用了html_simple_dom 这个封装的类，把期间各个栏目的文章转成dom对象，然后取节点，最后入库。

这里要讨论的如何将文件的数据插入[DEDECMS](http://www.pooy.net/category/network-programming/dedecms "DEDECMS")的表中，然后更新数据就能还原成一个个页面。

dedecms的文章关联的几个表是：

dede_addonarticle ：附加文章表
dede_arcatt ：文档自定义属性表
dede_arccache ：文档缓存表
dede_archives ：文档主表
dede_arcmulti ：多页标记存储数据表
dede_arcrank ：文档阅读权限表
dede_arctiny ：文档微表
dede_arctype ：栏目管理表
dede_channeltype ：自定义模型 ( 频道 )表

文章的绝大部分的信息都存在dede_archives ：文档主表中，文章的body存在dede_addonarticle ：附加文章表

文章的发表时间记录在dede_arctiny ：文档微表

他们是靠关联ID来实现多表联合查询的，所以插入的时候也要把条件分开插入。

具体写法可以参考：archives_add.php
<pre class="brush: php; gutter: true">//对保存的内容进行处理
 if(empty($writer)) $writer = $cuserLogin-&gt;getUserName();
 if(empty($source)) $source = &#039;未知&#039;;
 $pubdate = GetMkTime($pubdate);
 $senddate = time();
 $sortrank = AddDay($pubdate,$sortup);
 $ismake = $ishtml == 0 ? -1 : 0;
 $title = preg_replace(&quot;#\&quot;#&quot;, &#039;＂&#039;, $title);
 $title = cn_substrR($title,$cfg_title_maxlen);
 $shorttitle = cn_substrR($shorttitle,36);
 $color =  cn_substrR($color,7);
 $writer =  cn_substrR($writer,20);
 $source = cn_substrR($source,30);
 $description = cn_substrR($description,$cfg_auot_description);
 $keywords = cn_substrR($keywords,255);
 $filename = trim(cn_substrR($filename,40));
 $userip = GetIP();
 $isremote  = (empty($isremote)? 0  : $isremote);
 $voteid = (empty($voteid)? 0 : $voteid);
 $serviterm=empty($serviterm)? &quot;&quot; : $serviterm;
 if(!TestPurview(&#039;a_Check,a_AccCheck,a_MyCheck&#039;))
 {
 $arcrank = -1;
 }
 $adminid = $cuserLogin-&gt;getUserID();</pre>
插入sql：
<pre class="brush: php; gutter: true">$query = &quot;INSERT INTO `#@__archives`(id,typeid,typeid2,sortrank,flag,ismake,channel,arcrank,click,money,title,shorttitle, color,writer,source,litpic,pubdate,senddate,mid,voteid,notpost,description,keywords,filename,dutyadmin,weight) VALUES (&#039;$arcID&#039;,&#039;$typeid&#039;,&#039;$typeid2&#039;,&#039;$sortrank&#039;,&#039;$flag&#039;,&#039;$ismake&#039;,&#039;$channelid&#039;,&#039;$arcrank&#039;,&#039;$click&#039;,&#039;$money&#039;,&#039;$title&#039;,&#039;$shorttitle&#039;, &#039;$color&#039;,&#039;$writer&#039;,&#039;$source&#039;,&#039;$litpic&#039;,&#039;$pubdate&#039;,&#039;$senddate&#039;,&#039;$adminid&#039;,&#039;$voteid&#039;,&#039;$notpost&#039;,&#039;$description&#039;,&#039;$keywords&#039;,&#039;$filename&#039;,&#039;$adminid&#039;,&#039;$weight&#039;);
</pre>
保存到附加表
<pre class="brush: php; gutter: true">$cts = $dsql-&gt;GetOne(&quot;SELECT addtable FROM `#@__channeltype` WHERE id=&#039;$channelid&#039; &quot;);
 $addtable = trim($cts[&#039;addtable&#039;]);
 if(!empty($addtable))
 {
 $useip = GetIP();
 $query = &quot;INSERT INTO `{$addtable}`(aid,typeid,redirecturl,userip{$inadd_f}) Values(&#039;$arcID&#039;,&#039;$typeid&#039;,&#039;$redirecturl&#039;,&#039;$useip&#039;{$inadd_v})&quot;;
 if(!$dsql-&gt;ExecuteNoneQuery($query))
 {
 $gerr = $dsql-&gt;GetError();
 $dsql-&gt;ExecuteNoneQuery(&quot;DELETE FROM `#@__archives` WHERE id=&#039;$arcID&#039;&quot;);
 $dsql-&gt;ExecuteNoneQuery(&quot;DELETE FROM `#@__arctiny` WHERE id=&#039;$arcID&#039;&quot;);
 ShowMsg(&quot;把数据保存到数据库附加表 `{$addtable}` 时出错，请把相关信息提交给DedeCms官方。&quot;.str_replace(&#039;&quot;&#039;,&#039;&#039;,$gerr),&quot;javascript:;&quot;);
 exit();
 }
 }</pre>
这个就是把文章的内容添加到了附加表里面了。也就是html里面的body。

最后去更新下系统，自动就会生成静态的html文件即可。

DEDECMS5.7数据库下载地址：[http://www.pooy.net/down/dedecms5.7.doc](http://www.pooy.net/down/dedecms5.7.doc "dedecms5.7数据库结构说明文档")