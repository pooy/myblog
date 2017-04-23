---
title: DEDECMS自定义表单
tags:
  - DEDECMS
  - DEDECMS自定义表单
  - 自定义表单
  - 表单
id: 399
categories:
  - DEDECMS
  - 开源软件
date: 2012-10-26 23:18:31
---

DEDECMS在很多建站需求中，需要一些额外的表单供前台用户提交。以便于收集、统计、分析及处理更多的数据。比如：在线订单、在线报名等一些常见的互动应用。
我们以在线报名为例，展示如何自定义表单
1，在[DEDECMS](http://www.pooy.net/category/network-programming/dedecms "DEDECMS")[核心][频道模型][自定义表单]中增加一个自定义模型，两大步，一，创建自定义表单；二，创建自定义表单中需要的字段。
2，定义表单的调用，找到预览生成的表单html代码，将其复制到我们需要的区域。
3，查看发布内容，可以使用Loop标签。根据用到的模板进行更改。
4，演示如何在表单中增加验证码功能在表单中增加验证码字段，增加js的点击更换效果增加表单中关于验证码验证的代码，plus/diy.php中更改。
代码如下：
$svali = GetCkVdValue();

if(strtolower($captcha)!=$svali || $svali=='')
{
ResetVdValue();
ShowMsg('验证码错误！', '-1');
exit();
}