---
title: inspiron 5437（5438T）中如何win8改装win7
tags:
  - inspiron 5438T加内存，inspiron 5437换qin7
  - win8换win7系统
id: 1799
categories:
  - Windows
date: 2014-08-10 17:02:33
---

前两天给朋友拿了一台inspiron 5438T ，因为是做工程用，内存不太够用，于是就额外的订了一根金士顿 1600HZ 4G内存。

机器拿到后开机时win8初始化的界面，安装完毕后，关机。开始给inspiron 5438T安装内存条。内存的卡槽位于底部音响那边，只需要起一颗螺丝即可。开膛的时候需要用指甲把后盖翘起。取下后，直接上内存就好了。

Win8的系统实在是难用，就准备换系统，直接换成win7吧。过程有点崎岖，请听我一一道来：

因为之前一直没做过win8改win7还以为直接PE加GHOST就完事了，结果做下来发现不行。

于是修改BIOS模式:

开机F2进入，BIOS之后在BOOT选项里面 secure boot -&gt; Disabled。同时在下面的Boot list option 有两个选项 UEFI ，Legacy。选择Legacy即可。

准备一个[win7旗舰版镜像](http://www.pooy.net/windows7-down.html "WINDOWS7下载 WINDOWS7旗舰版下载"),如果有光盘&amp;刻录机那就刻录一张光盘，如果没有的话这年头总有个大于4G的U盘吧，做一个PE。

不会的请移步:[http://www.pooy.net/windows.html](http://www.pooy.net/windows.html "U盘做系统")

搞定了之后，那么重启机器，插入光盘或者U盘，按F12选择开机的启动项，如果是光盘就选择DVD，如果是U盘就选择U盘即可。

接下来就是一步步的开始安装系统了，有一步是需要删除原有的硬盘分区。删除就删除吧，反正是新机器里面空空如也。如果不是，请提前做好备份工作，不然都没了。

整个过程大概持续15~30分钟就完事了。

祝你好运！