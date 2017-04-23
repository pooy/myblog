---
title: 关于dedecms设置邮件发送的问题
tags:
  - DEDECMS
  - dedecms企业邮箱设置
  - Trying to smtp.qq.com
id: 1394
categories:
  - DEDECMS
  - 开源软件
date: 2013-08-04 19:30:12
---

在做邮件发送的时候遇到了一个问题，就是：

Trying to smtp.qq.com:465 Error: Remote host returned "" Error: Cannot connenct to relay host smtp.qq.com Error: (0) Error: Cannot send email to pooy@pooy.net

这个问题是说链接不到邮件服务器。我首先是按照腾讯的企业邮箱来做配置的。

[caption id="attachment_1395" align="aligncenter" width="717"][![](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱.png "dedecms配置腾讯企业邮箱")](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱.png) dedecms配置腾讯企业邮箱[/caption]

在后台，系统基本参数-》核心设置 里面设置如下：

[caption id="attachment_1396" align="aligncenter" width="794"][![](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱2.png "dedecms配置腾讯企业邮箱2")](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱2.png) dedecms配置腾讯企业邮箱2[/caption]

然后在后台参考/user/reg_new.php这个注册程序，写了一个test脚本：
<pre class="brush: php; gutter: false">	$to = &quot;pooy@pooy.net&quot;;
        $userhash = md5($cfg_cookie_encode.&#039;--&#039;.$mid.&#039;--&#039;.$email);
	$url = $cfg_basehost.(empty($cfg_cmspath) ? &#039;/&#039; : $cfg_cmspath).&quot;/member/index_do.php?fmdo=checkMail&amp;mid={$mid}&amp;userhash={$userhash}&amp;do=1&quot;;
	$url = preg_replace(&quot;#http:\/\/#i&quot;, &#039;&#039;, $url);
	$url = &#039;http://&#039;.preg_replace(&quot;#\/\/#&quot;, &#039;/&#039;, $url);
	$mailtitle = &quot;{$cfg_webname}--会员邮件验证通知&quot;;
	$mailbody = &#039;&#039;;
	$mailbody .= &quot;尊敬的用户[{$uname}]，您好：\r\n&quot;;
	$mailbody .= &quot;欢迎注册成为[{$cfg_webname}]的会员。\r\n&quot;;
	$mailbody .= &quot;要通过注册，还必须进行最后一步操作，请点击或复制下面链接到地址栏访问这地址：\r\n\r\n&quot;;
	$mailbody .= &quot;{$url}\r\n\r\n&quot;;
	$mailbody .= &quot;Power by pooy！\r\n&quot;;

	$headers = &quot;From: &quot;.$cfg_adminemail.&quot;\r\nReply-To: &quot;.$cfg_adminemail;

	if($cfg_sendmail_bysmtp == &#039;Y&#039; &amp;&amp; !empty($cfg_smtp_server))
	{        
		$mailtype = &#039;TXT&#039;;
		require_once(DEDEMINC.&#039;/mail.class.php&#039;);
		$smtp = new smtp($cfg_smtp_server,$cfg_smtp_port,true,$cfg_smtp_usermail,$cfg_smtp_password);
		$smtp-&gt;debug = true;
		$smtp-&gt;sendmail($to,$cfg_webname,$cfg_smtp_usermail, $mailtitle, $mailbody, $mailtype);

	}else{
		@mail($to, $webname, $subject, $headers);
	}</pre>
上面这段代码，里面利用了2中发送邮件的方式，第一种是利用/include/mail.class.php这个类来发送，第二种是利用系统的邮件系统发送。

执行之后，就开始报错：

Trying to smtp.qq.com:465 Error: Remote host returned "" Error: Cannot connenct to relay host smtp.qq.com Error: (0) Error: Cannot send email to pooy@pooy.net

然后使用：
<pre>@mail($to, $webname, $subject, $headers);</pre>
发现可以正常发送，只是有丁点慢。不过还能接受。然后就开始调试，1个小时无果，然后就打开foxmail添加该企业邮箱，发现了一个问题：

[caption id="attachment_1397" align="aligncenter" width="552"][![](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱3.png "dedecms配置腾讯企业邮箱3")](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱3.png) dedecms配置腾讯企业邮箱3[/caption]

&nbsp;

如果仔细查看，会发现foxmail默认导入的时候是使用的是：

服务器：smtp.qq.com

端口   ：25

而不是企业邮箱里面提示的：
<div>接收服务器：</div>
<div>**pop.exmail.qq.com(使用SSL，端口号995)**</div>
<div>发送服务器：</div>
<div>**smtp.exmail.qq.com(使用SSL，端口号465)**</div>
<div></div>
<div>我无语了，既然465端口有误，为什么还要提示我使用这个呢？修改之后再去执行那个测试脚本，一切正常！</div>
<div></div>
<div>邮件来了~</div>
<div>

[caption id="attachment_1398" align="aligncenter" width="674"][![](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱4.png "dedecms配置腾讯企业邮箱4")](http://www.pooy.net/wp-content/uploads/2013/08/dedecms配置腾讯企业邮箱4.png) dedecms配置腾讯企业邮箱4[/caption]

&nbsp;

所有出现Trying to smtp.qq.com:465 Error: Remote host returned  这个问题的时候，首先检查这个端口是否能用吧。

</div>