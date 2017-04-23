---
title: dedecms pagelist 去掉li
tags:
  - DEDECMS
  - dedecms pagelist 去掉li
id: 1360
categories:
  - DEDECMS
  - 开源软件
date: 2013-02-27 22:39:00
---

有两种情况，一种是文章列表页，一个是文章详情页。

首先说文章列表页：对应的类文件是：查找/include/arc.listview.class.php

按Ctrl+H键，查找

*   全部替换为空格
全部替换为空格

*   全部替换为空格
保存，覆盖原文件即可.列表页pagelist 去掉li 对应的类文件是：/include/arc.archive.class.php,操作如上即可！

    如下图：

    [![](http://www.pooy.net/wp-content/uploads/2013/07/Image.png "Image")](http://www.pooy.net/wp-content/uploads/2013/07/Image.png)
[caption id="attachment_1362" align="aligncenter" width="558"][![](http://www.pooy.net/wp-content/uploads/2013/07/Imagepage.png "Imagepage")](http://www.pooy.net/wp-content/uploads/2013/07/Imagepage.png) dedecms pagelist 去掉li[/caption]