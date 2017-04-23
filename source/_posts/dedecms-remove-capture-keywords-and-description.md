---
title: dedecms去掉采集关键字和描述
tags:
  - DEDECMS
  - dedecms去掉采集关键字和描述
  - dedecms怎么采集
id: 1262
categories:
  - DEDECMS
  - 开源软件
date: 2013-04-20 21:14:48
---

使用[dedecms采集](http://www.pooy.net/dedecms.html "dedecms采集的一些采集规律")的时候，有时候系统会自动去采集目标页面的keyword跟description。但是有时候就偏偏事与愿违，不需要那些东西。所以就得过滤。

首先想到的是直接在处理[dedecms采集](http://www.pooy.net/dedecms.html "dedecms采集的一些采集规律")的文件co_edit.php里面直接降结果直接滞空，不过仔细想想如果下一个采集规则需要呢？最后还是没有这么干。因为dedecms提供了自带的过滤方法。

[caption id="attachment_1263" align="aligncenter" width="364"][![ dedecms采集](http://www.pooy.net/wp-content/uploads/2013/04/dedecms采集.jpg "dedecms采集")](http://www.pooy.net/wp-content/uploads/2013/04/dedecms采集.jpg) dedecms采集[/caption]

这些过滤的方法都是基于正则的，最后使用了：
<pre class="brush: php; gutter: true">{dede:trim replace=&#039;&#039;}|*|{/dede:trim}</pre>
将所有的替换成空就好了：

[caption id="attachment_1264" align="aligncenter" width="515"][![](http://www.pooy.net/wp-content/uploads/2013/04/dedecms采集过滤关键字跟描述.jpg "dedecms采集过滤关键字跟描述")](http://www.pooy.net/wp-content/uploads/2013/04/dedecms采集过滤关键字跟描述.jpg) dedecms采集过滤关键字跟描述[/caption]

&nbsp;

<span style="color: #ff0000;">注意看清，这里是两个“｜”中间一个“*”。</span>

最后效果如下：

[caption id="attachment_1265" align="aligncenter" width="673"][![](http://www.pooy.net/wp-content/uploads/2013/04/dedecms采集过滤关键字跟描述2.jpg "dedecms采集过滤关键字跟描述2")](http://www.pooy.net/wp-content/uploads/2013/04/dedecms采集过滤关键字跟描述2.jpg) dedecms采集过滤关键字跟描述[/caption]

&nbsp;

如果需要把关键字或者描述里面的某些字符替换成自己想要的，可以使用：
<pre class="brush: php; gutter: false">{dede:trim replace=&#039;你要替换成的字符&#039;}你要被替换的字符{/dede:trim}</pre>
这就是dedecms去掉采集关键字和描述的方法，就到这里吧！

之前写过一篇《[dedecms采集的一些采集规律](http://www.pooy.net/dedecms.html "链接到 dedecms采集的一些采集规律")》，有兴趣可以看看！