---
title: ImageMagick 中文手册
tags:
  - imagemagick
  - imagemagick linux
  - imagemagick 中文手册
  - imagemagick 命令
  - imagemagick 裁剪
id: 720
categories:
  - Imagemagick
  - 开源软件
  - 手册资料
date: 2012-12-14 23:00:34
---

<div>

PS：写了一个imagemagick命令里面的convert（裁剪）跟composite（加水印）的压力测试脚本，就是因为有一台服务器身兼多职。最近负载有点过高，所以要考虑，把这个功能单独拿出去，做一个专门裁剪图片的服务器！现在把imagemagick 在linux下imagemagick 裁剪 命令中文手册发一遍方便大家使用！
在认识ImageMagick之前，我使用的图像浏览软件是KuickShow，截图软件是KSnapShot，这两款软件都是KDE附带的软件，用起来也是蛮方便的。在一次偶然的机会中，我遇到了ImageMagick，才发现Linux竟然有如此功能强大的图像软件。

你将会发现，大部分的操作，你只要在终端下动动键盘即可，省得你用鼠标点来点去。

下面，我对ImageMagick的主要功能做一个简单的介绍，其中覆盖的大都是人们常用的一些功能，如果你要全面的了解它的知识，你可以看看它的man手册。

## convert

convert顾名思义就是对图像进行转化，它主要用来对图像进行格式的转化，同时还可以做缩放、剪切、模糊、反转等操作。

*   格式转化
*   比如把 foo.jpg 转化为 foo.png：
<pre>convert foo.jpg foo.png</pre>
如果要想把目录下所有的jpg文件都转化为gif，我们可借助于shell的强大功能：
<pre>find ./ -name &quot;*.jpg&quot; -exec convert {} {}.gif \;</pre>
转化后的gif名称为 *.jpg.gif ，这样看起来不太自然，没关系，我们可以再来一步：
<pre>rename .jpg.gif .gif *.jpg.gif</pre>
本来，我想在find的时候，用basename来取得不带后缀的文件名的，这样就不会形成.jpg.gif这种丑陋的名子了，可是不知道为什么，就是不行，如果你知道的话，告诉我或者，你也可用shell script来完成上述的操作：
<pre>for i in *.jpg
do
convert $i `basename $i .jpg`.gif
done</pre>
我们还可用mogrify来完成同样的效果：
<pre>mogrify -format png *.jpg</pre>
上面命令将会把目录下面所有的jpg文件转化为png格式。convert还可以把多张照片转化成pdf格式：
<pre>convert *.jpg foo.pdf</pre>

*   大小缩放
*   比如我们要为一个普通大小的图片做一个缩略图，我们可以这样
<pre>convert -resize 100x100 foo.jpg thumbnail.jpg</pre>
你也可以用百分比，这样显的更为直观：
<pre>convert -resize 50%x50% foo.jpg thumbnail.jpg</pre>
convert会自动地考虑在缩放图像大小时图像的高宽的比例，也就是说着新的图像的高宽比与原图相同。我们还可以批量生成缩略图：
<pre>mogrify -sample 80x60 *.jpg</pre>
注意，这个命令会覆盖原来的图片，不过你可以在操作前，先把你的图片备份一下。
*   加边框
*   在一张照片的四周加上边框，可以用 -mattecolor 参数，比如某位同志牺牲了，我们需要为他做一张黑边框的遗像，可以这样：
<pre>convert -mattecolor &quot;#000000&quot; -frame 60x60 yourname.jpg rememberyou.png</pre>
其中，"#000000"是边框的颜色，边框的大小为60x60你也可以这样加边框:
<pre>convert -border 60x60 -bordercolor &quot;#000000&quot; yourname.jpg rememberyou.png</pre>

*   在图片上加文字
*   <pre>convert -fill green -pointsize 40 -draw &#039;text 10,50 &quot;charry.org&quot;&#039; foo.png bar.png</pre>
上面的命令在距离图片的左上角10x50的位置，用绿色的字写下charry.org，如果你要指定别的字体，可以用-font参数。
*   模糊
*   高斯模糊:
<pre>convert -blur 80 foo.jpg foo.png</pre>
-blur参数还可以这样-blur 80x5。后面的那个5表示的是Sigma的值，这个是图像术语，我也不太清楚，总之，它的值对模糊的效果起关键的作用。
*   翻转
*   上下翻转：
<pre>convert -flip foo.png bar.png</pre>
左右翻转：
<pre>convert -flop foo.png bar.png</pre>

*   反色
*   形成底片的样子：
<pre>convert -negate foo.png bar.png</pre>

*   单色
*   把图片变为黑白颜色：
<pre>convert -monochrome foo.png bar.png</pre>

*   加噪声
*   <pre>convert -noise 3 foo.png bar.png</pre>

*   油画效果
*   我们可用这个功能，把一张普通的图片，变成一张油画，效果非常的逼真
<pre>convert -paint 4 foo.png bar.png</pre>

*   旋转
*   把一张图片，旋转一定的角度：
<pre>convert -rotate 30 foo.png bar.png</pre>
上面的30，表示向右旋转30度，如果要向左旋转，度数就是负数。
*   炭笔效果
*   <pre>convert -charcoal 2 foo.png bar.png</pre>
形成炭笔或者说是铅笔画的效果。
*   散射
*   毛玻璃效果：
<pre>convert -spread 30 foo.png bar.png</pre>

*   漩涡
*   以图片的中心作为参照，把图片扭转，形成漩涡的效果：
<pre>convert -swirl 67 foo.png bar.png</pre>

*   凸起效果
*   用-raise来创建凸边：
<pre>convert -raise 5x5 foo.png bar.png</pre>
执行后，你会看到，照片的四周会一个5x5的边，如果你要一个凹下去的边，把-raise改为+raise就可以了。其实凸边和凹边看起来区别并不是很大。
*   其他
*   其他功能都是不太常用的，如果你感兴趣的话，可以看它的联机文档

## import

import是一个用于屏幕截图的组件，下面列出的是我们常用的功能，其他的功能，你参考它的man好了。

*   截取屏幕的任一矩形区域
*   <pre>import foo.png</pre>
在输入上述的命令后，你的鼠标会变成一个十字，这个时候，你只要在想要截取的地方划一个矩形就可以了
*   截取程序的窗口
*   <pre>import -pause 3 -frame foo.png</pre>
回车后，用鼠标在你想截的窗口上点一下即可。参数 -frame的作用是告诉import，截图的时候把目标窗口的外框架带上，参数-pause的作用很重要，你可以试着把它去掉，对比一下，你会发现，目 标窗口的标题栏是灰色的，pause就是让import稍微延迟一下，等你的目标窗口获得焦点了，才开始截图，这样的图才比较自然。
*   截取一个倾斜的窗口
*   如果想让你的截图比较cool，你可以把截取一个倾斜的窗口，方法如下：
<pre>import -rotate 30 -pause 3 -frame foo.png</pre>

*   截取整个屏幕
*   <pre>import -pause 3 -window root screen.png</pre>
注意，暂停了3秒钟，你需要在3秒钟内切换到需要截取的画面噢。

## display

display应该是我们使用的最为频繁的图像处理软件了，毕竟，还是看的多

*   显示图片
*   <pre>display foo.png</pre>
如果你要显示多个文件，你可以使用通配符
<pre>display *.png</pre>

*   幻灯片
*   <pre>display -delay 5 *</pre>
每隔5个百分之秒显示一张图片
*   一些快捷键
*   1.  space(空格): 显示下一张图片
    2.  backspace(回删键):显示上一张图片
    3.  h: 水平翻转
    4.  v: 垂直翻转
    5.  /:顺时针旋转90度
    6.  \:逆时针旋转90度
    7.  &gt;: 放大
    8.  &lt;: 缩小
    9.  F7:模糊图片
    10.  Alt+s:把图片中间的像素旋转
    11.  Ctrl+s:图象另存
    12.  Ctrl+d:删除图片
    13.  q: 退出

## 其他

ImageMagick还提供有丰富的编程接口，比如，你可以用php来调用它，用ImageMagick来生成验证码图片，效果非常棒。

ImageMagick还有一个小工具identify，它可以用来显示一个图片文件的详悉信息，比如格式、分辨率、大小、色深等等，你都可用它来帮你的忙。

如果你对命令行不太熟悉，你也可以在图片上单击，你会发现，通过鼠标你也可以完成图像的编辑。
<div>ImageMagick的网站：[www.imagemagick.org](http://www.imagemagick.org/)。[这里](http://www.imagemagick.org/image/examples.jpg)是ImageMagick加工过的图片的例子。</div>
<div></div>
<div>原文地址：http://www.charry.org/docs/linux/ImageMagick/ImageMagick.html</div>
</div>