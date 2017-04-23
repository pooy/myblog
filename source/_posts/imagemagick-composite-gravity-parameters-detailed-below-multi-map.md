---
title: ImageMagicK下composite的gravity参数详解（多图）
tags:
  - composite
  - featured
  - imagemagick
  - ImageMagicK gravity
id: 1621
categories:
  - Imagemagick
  - 开源软件
date: 2013-11-02 18:31:19
---

<div>
<div>在使用[**imagemagick**](http://www.pooy.net/category/os/imagemagick-os "imagemagick")的时候，使用**gravity**重新定义坐标后，可以很容易让子元素与父元素的对齐方式达到想要的效果，让一切变得非常简单。比如把一张小图片叠加到背景图片的 正中位置，按照默认的坐标系统，那必须要先知道背景图片和小图片的宽度以及高度，然后才能计算出起始点的坐标，再通过-geometry来设置坐标点。</div>
<div></div>
<div>如 果使用gravity，把其设置center，即把中心作为坐标的原点，那么根本不需要计算起始坐标点，ImageMagicK会自动把小图片放置在背景 的正中央位置，-geometry默认是+0+0。</div>
<div></div>
<div>gravity不仅影响父元素的坐标系统**，而且子元素的重心点（或者叫参照点）也随之改变**。举例来说，当gravity值为southeast，父元素的坐标原点变为右下角了，x轴方向是从右到左，y轴方向从下到上；子元素重心点也是右下角，所以geometry设置的坐标点就是子元素的右下角相对父元素右下角的位置。**gravity会影响通过geometry、annotate、region等来定义坐标点。**</div>
<div>**
**</div>
gravity可用值有九个,分别是:

*   **NorthWest：**左上角为坐标原点，x轴从左到右，y轴从上到下，也是默认值。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity northwest -geometry +10+10 -composite -size 100x50 xc:yellow -gravity northwest -geometry +110+60 -composite -gravity northwest -fill green -pointsize 24 -draw "text 110,60 ' www.pooy.net '" dome1.png</div>
**说明：**创建一个灰色的400×120的背景,分别把两个100×50的小图片放置在背景上的(10,10)和(110,60)的位置,同时通过draw在图片输入一段文本，小图片和文本的参照点是左上角，效果如下图。

[caption id="attachment_1622" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome1.png "dome1")](http://www.pooy.net/wp-content/uploads/2013/11/dome1.png) dome1[/caption]

*   **North：**上部中间位置为坐标原点，x轴从左到右，y轴从上到下。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity north -geometry +10+10 -composite -size 100x50 xc:yellow -gravity north -geometry +110+60 -composite -fill green -pointsize 24 -gravity north -draw "text 0,60 ' www.pooy.net '" dome2.png</div>
**说明：**小图片和文字的参照点是上部中间位置，效果如下图。

[caption id="attachment_1623" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome2.png "dome2")](http://www.pooy.net/wp-content/uploads/2013/11/dome2.png) dome2[/caption]

*   **NorthEast：**右上角位置为坐标原点，x轴从右到左，y轴从上到下。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity NorthEast -geometry +10+10 -composite -size 100x50 xc:yellow -gravity NorthEast -geometry +110+60 -composite -fill green -pointsize 24 -gravity NorthEast -draw "text 10,60 ' www.pooy.net '" dome3.png</div>
**说明：**小图片和文字的参照点是右上角位置，效果如下图。

[caption id="attachment_1624" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome3.png "dome3")](http://www.pooy.net/wp-content/uploads/2013/11/dome3.png) dome3[/caption]

&nbsp;

*   **West：**左边缘中间位置为坐标原点，x轴从左到右，y轴从上到下。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity West -geometry +10+10 -composite -size 100x50 xc:yellow -gravity West -geometry +110-40 -composite -fill green -pointsize 24 -gravity West -draw "text 0,0 ' www.pooy.net '" dome4.png</div>
**说明：**小图片和文字的参照点是左边缘的中间位置，效果如下图。

[caption id="attachment_1625" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome4.png "dome4")](http://www.pooy.net/wp-content/uploads/2013/11/dome4.png) dome4[/caption]

*   **Center：**正中间位置为坐标原点，x轴从左到右，y轴从上到下。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity Center -geometry +50+25 -composite -size 100x50 xc:yellow -gravity Center -geometry -50-25 -composite -fill green -pointsize 24 -gravity Center -draw "text 0,0 ' www.pooy.net '" dome5.png</div>
**说明：**小图片和文字的参照点是正中间的位置，效果如下图。

[caption id="attachment_1626" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome5.png "dome5")](http://www.pooy.net/wp-content/uploads/2013/11/dome5.png) dome5[/caption]

&nbsp;

*   **East：**右边缘的中间位置为坐标原点，x轴从右到左，y轴从上到下。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity East -geometry +10+25 -composite -size 100x50 xc:yellow -gravity East -geometry +110-25 -composite -fill green -pointsize 24 -gravity East -draw "text 10,0 ' www.pooy.net '" dome6.png</div>
**说明：**小图片和文字的参照点是右边缘中间的位置，效果如下图。

[caption id="attachment_1627" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome6.png "dome6")](http://www.pooy.net/wp-content/uploads/2013/11/dome6.png) dome6[/caption]

&nbsp;

*   **SouthWest：**左下角为坐标原点，x轴从左到右，y轴从下到上。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity SouthWest -geometry +10+10 -composite -size 100x50 xc:yellow -gravity SouthWest -geometry +110+60 -composite -fill green -pointsize 24 -gravity SouthWest -draw "text 10,10 ' www.pooy.net '" dome7.png</div>
<div></div>
<div>**说明：**小图片和文字的参照点是左下角的位置，效果如下图。</div>
<div>

[caption id="attachment_1628" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome7.png "dome7")](http://www.pooy.net/wp-content/uploads/2013/11/dome7.png) dome7[/caption]

&nbsp;

</div>
<div></div>

*   **South：**下边缘的中间为坐标原点，x轴从左到右，y轴从下到上。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity South -geometry +10+10 -composite -size 100x50 xc:yellow -gravity South -geometry +110+60 -composite -fill green -pointsize 24 -gravity South -draw "text 0,0 ' www.pooy.net '" dome8.png</div>
<div></div>
<div>**说明：**小图片和文字的参照点是下边缘的中间，效果如下图。</div>
<div>

[caption id="attachment_1629" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome8.png "dome8")](http://www.pooy.net/wp-content/uploads/2013/11/dome8.png) dome8[/caption]

&nbsp;

</div>
<div></div>
<div></div>

*   **SouthEast：**右下角坐标原点，x轴从右到左，y轴从下到上。
<div>convert -size 400x120 xc:gray -size 100x50 xc:blue -gravity SouthEast -geometry +10+10 -composite -size 100x50 xc:yellow -gravity SouthEast -geometry +110+60 -composite -fill green -pointsize 24 -gravity SouthEast -draw "text 0,0 ' www.pooy.net '" dome9.png</div>
<div></div>
<div>

**说明：**小图片和文字的参照点右下角，效果如下图。

[caption id="attachment_1630" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2013/11/dome9.png "dome9")](http://www.pooy.net/wp-content/uploads/2013/11/dome9.png) dome9[/caption]

</div>
<div></div>
<div></div>
<div>资料来源于网上。</div>
</div>
<div></div>
<div>更多imagemagick资料请移步：[http://www.pooy.net/category/os/imagemagick-os](http://www.pooy.net/category/os/imagemagick-os "imagemagick资料")</div>