---
title: linux screen 命令大全
tags:
  - linux
  - linux screen 命令大全
  - screen
  - screen 命令文档
id: 201
categories:
  - Linux
  - 手册资料
date: 2012-08-08 22:50:44
---

<div>linux screen 命令详解</div>
<div></div>
**功能说明： **

使 用telnet或SSH远程登录linux时，如果连接非正常中断，重新连接时，系统将开一个新的session，无法恢复原来的 session.screen命令可以解决这个问题。Screen工具是一个终端多路转接器，在本质上，这意味着你能够使用一个单一的终端窗口运行多终端 的应用。

**语　　法：**
<pre class="brush: bash; gutter: true">screen [-AmRvx -ls -wipe][-d &lt;作业名称&gt;][-h &lt;行数&gt;][-r &lt;作业名称&gt;][-s ][-S &lt;作业名称&gt;]</pre>
**补充说明：**

screen为多重视窗管理程序。此处所谓的视窗，是指一个全屏幕的文字模式画面。通常只有在使用telnet登入主机或是使用老式的终端机时，才有可能用到screen程序。

**参　　数：**
<pre class="brush: bash; gutter: true">-A 　将所有的视窗都调整为目前终端机的大小。
 -d &lt;作业名称&gt; 　将指定的screen作业离线。
 -h &lt;行数&gt; 　指定视窗的缓冲区行数。
 -m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
 -r &lt;作业名称&gt; 　恢复离线的screen作业。
 -R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
 -s 　指定建立新视窗时，所要执行的shell。
 -S &lt;作业名称&gt; 　指定screen作业的名称。
 -v 　显示版本信息。
 -x 　恢复之前离线的screen作业。
 -ls或--list 　显示目前所有的screen作业。
 -wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。</pre>
**常用screen参数：**
<pre class="brush: bash; gutter: true">screen -S yourname -&gt; 新建一个叫yourname的session
 screen -ls -&gt; 列出当前所有的session
 screen -r yourname -&gt; 回到yourname这个session
 screen -d yourname -&gt; 远程detach某个session
 screen -d -r yourname -&gt; 结束当前session并回到yourname这个session</pre>
**在每个screen session 下，所有命令都以 ctrl+a(C-a) 开始。**
<pre class="brush: bash; gutter: true">C-a ? -&gt; Help，显示简单说明
 C-a c -&gt; Create，开启新的 window
 C-a n -&gt; Next，切换到下个 window
 C-a p -&gt; Previous，前一个 window
 C-a 0..9 -&gt; 切换到第 0..9 个window
 Ctrl+a [Space] -&gt; 由視窗0循序換到視窗9
 C-a C-a -&gt; 在两个最近使用的 window 间切换
 C-a x -&gt; 锁住当前的 window，需用用户密码解锁
 C-a d -&gt; detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows) 丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里    每个 window 内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。
 C-a z -&gt; 把当前session放到后台执行，用 shell 的 fg 命令則可回去。
 C-a w -&gt; Windows，列出已开启的 windows 有那些
 C-a t -&gt; Time，显示当前时间，和系统的 load
 C-a K -&gt; kill window，强行关闭当前的 window
 C-a [ -&gt; 进入 copy mode，在 copy mode 下可以回滚、搜索、复制就像用使用 vi 一样
 C-b Backward，PageUp
 C-f Forward，PageDown
 H(大写) High，将光标移至左上角
 L Low，将光标移至左下角
 0 移到行首
 $ 行末
 w forward one word，以字为单位往前移
 b backward one word，以字为单位往后移
 Space 第一次按为标记区起点，第二次按为终点
 Esc 结束 copy mode
 C-a ] -&gt; Paste，把刚刚在 copy mode 选定的内容贴上</pre>
退出一个screen，直接输入<span style="color: #ff0000;">“exit” </span>即可！

官方手册：[http://www.gnu.org/software/screen/manual/screen.html#Commands](http://www.gnu.org/software/screen/manual/screen.html#Commands "screen-Commands")

参考原文：[http://bjzero.blogbus.com/logs/30983025.html](http://bjzero.blogbus.com/logs/30983025.html)