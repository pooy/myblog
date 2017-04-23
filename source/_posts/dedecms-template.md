---
title: DEDECMS模板使用小总结
tags:
  - DEDECMS
  - dedecms模板使用 dedecms模板下载
id: 186
categories:
  - DEDECMS
  - 开源软件
date: 2012-07-04 08:22:05
---

上次说到了dedecms采集的一个规律，以及dedecms与discuz的ucenter整合问题，今天说下dedecms模板的使用。

**模板使用小总结：**

1，{dede:标签 属性=”属性值” .. }

Innertext(底层模板)

{/dede::标签}

2，{dede:标签 /}

&nbsp;

如果包含了底层模板，将使用底层模板中的样式显示我们的数据

如果没有包含，将使用系统默认的模板完成数据的显示