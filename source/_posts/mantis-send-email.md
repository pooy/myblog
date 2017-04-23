---
title: 解决mantis无法发送邮件的问题
tags:
  - mantis发邮件
  - mantis无法发送邮件
  - mantis邮件发送
  - mantis邮件提醒
  - mantis邮件设置
id: 1755
categories:
  - Linux
  - PHP
  - 开源软件
date: 2014-04-07 17:14:54
---

Mantis安装好之后，注册用户的时候，发送用户邮件不太好使。通过查看Mantis源码，调试mantis邮件配置后解决了这个的问题。

1，**测试发送邮件**
需要修改的文件：<span style="text-decoration: underline;">config_defaults_inc.php</span>。下面以腾讯企业邮箱为例
<pre class="brush: php; gutter: true">     $g_administrator_email     = &#039;mantis@pooy.net&#039;;
     $g_webmaster_email          = &#039;mantis@pooy.net&#039;;
     $g_from_email               = &#039;mantis@pooy.net&#039;;
     $g_return_path_email     = &#039;mantis@pooy.net&#039;;
     $g_enable_email_notification     = ON;
     $g_phpMailer_method        = PHPMAILER_METHOD_SMTP;
     $g_smtp_host               = &#039;smtp.exmail.qq.com&#039;;
     $g_smtp_username = &#039;mantis@pooy.net&#039;;
     $g_smtp_password = &#039;JD8WJ9KD&#039;;
     $g_smtp_port = 465;</pre>
配置文件修改完毕之后，接着开始测试：mantis里面自带有测试文件，目录在：<span style="text-decoration: underline;">library/phpmailer/email.php</span>
我的测试脚本如下：
<pre class="brush: php; gutter: true">&lt;?php
/**
 * @author [pooy] &lt;[pooy@pooy.net]&gt;
 * @blog  http://www.pooy.net
 */
require &#039;class.phpmailer.php&#039;;

$mail = new PHPMailer;
$mail-&gt;SMTPDebug = 1;
$mail-&gt;IsSMTP();                                      // Set mailer to use SMTP

$mail-&gt;Host = &#039;smtp.exmail.qq.com&#039;;  // Specify main and backup server
$mail-&gt;SMTPAuth = true;                               // Enable SMTP authentication
$mail-&gt;Username = &#039;mantis@pooy.net&#039;;                            // SMTP username
$mail-&gt;Port = 465;
$mail-&gt;Password = &#039;JD8WJ9KD&#039;;                           // SMTP password
$mail-&gt;SMTPSecure = &#039;ssl&#039;;                            // Enable encryption, &#039;ssl&#039; also accepted
$mail-&gt;From = &#039;mantis@pooy.net&#039;;

$mail-&gt;FromName = &#039;Mailer Testing&#039;;

$mail-&gt;WordWrap = 50;                                 // Set word wrap to 50 characters
$mail-&gt;IsHTML(true);                                  // Set email format to HTML

$mail-&gt;Subject = &#039;Here is the subject&#039;;
$mail-&gt;Body    = &#039;This is the HTML message body &lt;b&gt;in bold!&lt;/b&gt;&#039;;
$mail-&gt;AltBody = &#039;This is the body in plain text for non-HTML mail clients&#039;;

if(!$mail-&gt;Send()) {
   echo &#039;Message could not be sent.&#039;;
   echo &#039;Mailer Error: &#039; . $mail-&gt;ErrorInfo;
   exit;
}

echo &#039;Message has been sent&#039;;
// To load the French version
$mail-&gt;SetLanguage(&#039;cn&#039;, &#039;/optional/path/to/language/directory/&#039;);
?&gt;</pre>
你可以使用浏览器访问这个脚本，或者直接在shell或者dos下运行这个。
因为上面我开了debug，所以会得到下面的返回结果：
<pre class="brush: bash; gutter: true">CLIENT -&gt; SMTP: EHLO 115.28.15.50
CLIENT -&gt; SMTP: AUTH LOGIN
CLIENT -&gt; SMTP: bWFudGlzQHdlaXRvdWh1aXJvbmcuY29t
CLIENT -&gt; SMTP: SkQ4V0o5S0Q=
CLIENT -&gt; SMTP: MAIL FROM:
CLIENT -&gt; SMTP: RCPT TO:
CLIENT -&gt; SMTP: DATA
CLIENT -&gt; SMTP: Date: Wed, 7 May 2014 16:34:36 +0800
CLIENT -&gt; SMTP: Return-Path:
CLIENT -&gt; SMTP: To: Pooy
CLIENT -&gt; SMTP: From: Mailer Testing
CLIENT -&gt; SMTP: Subject: Here is the subject
CLIENT -&gt; SMTP: Message-ID:
CLIENT -&gt; SMTP: X-Priority: 3
CLIENT -&gt; SMTP: X-Mailer: PHPMailer 5.2.6 (https://github.com/PHPMailer/PHPMailer/)
CLIENT -&gt; SMTP: MIME-Version: 1.0
CLIENT -&gt; SMTP: Content-Type: multipart/alternative;
CLIENT -&gt; SMTP: boundary=&quot;b1_d5e58d84c060fd873a3452afc752ca93&quot;
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP: --b1_d5e58d84c060fd873a3452afc752ca93
CLIENT -&gt; SMTP: Content-Type: text/plain; charset=iso-8859-1
CLIENT -&gt; SMTP: Content-Transfer-Encoding: 8bit
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP: This is the body in plain text for non-HTML mail
CLIENT -&gt; SMTP: clients
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP: --b1_d5e58d84c060fd873a3452afc752ca93
CLIENT -&gt; SMTP: Content-Type: text/html; charset=iso-8859-1
CLIENT -&gt; SMTP: Content-Transfer-Encoding: 8bit
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP: This is the HTML message body in bold!
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP: --b1_d5e58d84c060fd873a3452afc752ca93--
CLIENT -&gt; SMTP:
CLIENT -&gt; SMTP: .
CLIENT -&gt; SMTP: quit
Message has been sent</pre>
到这里，说明测试邮件发送无误！

**2，注册发送邮件**

用SMTP发送邮件时，发送进程比较慢，如果参数<span style="text-decoration: underline;">g_email_send_using_cronjob</span>设置为OFF，则MantisBT的处理脚本就要 等待邮件发送进程结束才会返回，这样用户往往要等待很久才能看到页面的反馈，用户体验很差。当上述参数设置为ON时，MantisBT会将待发送通知邮件 放入邮件队列，等待定时进程发送。
定时脚本在：
<pre class="brush: bash; gutter: true">php /home/yourusername/yourpath/mantis/scripts/send_emails.php</pre>
可以放在crontab里面一分钟执行一次
#Mantis  Sending  emailList
<pre class="brush: bash; gutter: true">*/1 * * * *  php /data/www/mantis/scripts/send_emails.php  &gt;/dev/null</pre>
调试的时候，第一次可能会出现：
<pre class="brush: bash; gutter: true">sh: /var/qmail/bin/sendmail: No such file or directory
sh: /var/qmail/bin/sendmail: No such file or directory
Sending emails...
Done.</pre>
原因是因为config_defaults_inc.php文件中的:
<pre class="brush: php; gutter: true">$g_phpMailer_method          =PHPMAILER_METHOD_SENDMAIL;</pre>
在一开始我提过要修改成：
<pre class="brush: php; gutter: true">$g_phpMailer_method          = PHPMAILER_METHOD_SMTP;</pre>
那样就可以使用class.phpmailer.php了。
最后还需要修改emil_api.php中的 email_send 函数中的：
<pre class="brush: php; gutter: true">if( is_null( $g_phpMailer ) ) {
          if ( $t_mailer_method == PHPMAILER_METHOD_SMTP ) {
               register_shutdown_function( &#039;email_smtp_close&#039; );
          }
          $mail = new PHPMailer(true);
          /**
          * 添加发送邮件的参数
          * @author [pooy] &lt;[pooy@pooy.net]&gt;
          */
          $mail-&gt;SMTPDebug = 1;          //生产环境中去掉
          $mail-&gt;SMTPAuth = true;        // Enable SMTP authentication
          $mail-&gt;Port = 465;            
          $mail-&gt;SMTPSecure = &#039;ssl&#039;;      
     } else {
          $mail = $g_phpMailer;
     }</pre>
到这里，就可以自动完成定时发送邮件的服务了。这里顺便说一下mantis发送邮件的原理：定时任务中的send_emails.php，会读取数据库的邮件队列表mantis_email_table.获取发送的列表后，依次按照ID的升序发送，发送完毕后清空。等待下次队列，周而复始。

关键字：mantis邮件设置,mantis邮件提醒,mantis邮件发送,mantis发邮件,mantis无法发送邮件