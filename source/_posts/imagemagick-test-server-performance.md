---
title: imagemagick批量裁图 imagemagick测试服务器性能
tags:
  - imagemagick
  - imagemagick命令
  - imagemagick安装
  - imagemagick批量裁图
  - shell
id: 711
categories:
  - Imagemagick
  - 开源软件
date: 2012-12-13 23:16:32
---

[caption id="attachment_715" align="aligncenter" width="400"][![](http://www.pooy.net/wp-content/uploads/2012/12/imagemagick.jpg "imagemagick")](http://www.pooy.net/wp-content/uploads/2012/12/imagemagick.jpg) imagemagick[/caption]

今天写了一个imagemagick命令里面的convert（裁剪）跟composite（加水印）的压力测试脚本，就是因为有一台服务器身兼多职。最近负载有点过高，所以要考虑，把这个功能单独拿出去，做一个专门裁剪图片的服务器！

下面是写的imagemagick里面的convert（裁剪）跟composite（加水印）的压力测试脚本，这个imagemagick批量裁图脚本放在你要裁剪的文件的文件夹中即可：
<div>
<pre class="brush: bash; gutter: true">#!/bin/sh
#Get Convert &amp; Composite Time

#Computing time
function getTiming(){
    start=$1
    end=$2

    start_s=`echo $start | cut -d &#039;.&#039; -f 1`
    start_ns=`echo $start | cut -d &#039;.&#039; -f 2`
    end_s=`echo $end | cut -d &#039;.&#039; -f 1`
    end_ns=`echo $end | cut -d &#039;.&#039; -f 2`

    time_micro=$(( (10#$end_s-10#$start_s)*1000000 + (10#$end_ns/1000 - 10#$start_ns/1000) ))
    time_ms=`expr $time_micro/1000  | bc `

    echo &quot;$time_micro microseconds&quot;
    echo &quot;$time_ms ms&quot;
}

begin_time=`date +%s.%N`

for i in *.JPG
do
/usr/bin/convert -size 550x550 $i -resize 550x550 +profile &quot;*&quot; -quality 80 $i
composite -gravity Center -compose plus /data/shuiyin.jpg $i  /data/mdwaterimg/test2/$i
done

end_time=`date +%s.%N`

getTiming $begin_time $end_time</pre>
</div>
之前考虑过不用shell写，直接用PHP来写。但是用PHP写的话，测试裁剪加水印的时间数据就不会那么准确！

执行：
<pre class="brush: bash; gutter: true">sh  /imagemagick_test.sh</pre>
上面这段代码里面包含一个记录裁剪加水印的开始时间，跟结束时间，返回的是毫秒。
<div>

对不同单位的时间有所混淆么？下面是备忘。

1s=1000ms

1ms=1000 microseconds

</div>
&nbsp;