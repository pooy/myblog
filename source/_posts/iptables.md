---
title: iptables详讲 菜鸟的突破
tags:
  - iptables
  - iptables使用方法
  - iptables设置
id: 428
categories:
  - Linux
  - 手册资料
  - 防火墙
date: 2012-04-03 23:26:07
---

## iptables

之前对iptables只有一个概念行的东西，只是说作为一个防火墙。没有好好的去研究下究竟。

今天周末，花了一天的时间来学习这个东东，自己用Ubuntu 12.0.10的服务器版做的。

总的来说收获不错，所以贴出来大家一起学习学习。

百度百科对iptables的解释：iptables 是与最新的 3.5 版本 Linux 内核集成的 IP 信息包过滤系统。如果 Linux 系统连接到因特网或 LAN、服务器或连接 LAN 和因特网的代理服务器， 则该系统有利于在 Linux 系统上更好地控制 IP 信息包过滤和防火墙配置。

&nbsp;

简单的说，为了我们的系统更加安全，稳定。所以要做好防火墙，iptables在防火墙里面做的很出色。所以无可厚非的要去学习她！“IPTABLES”！

**第一，      ****首先要介绍下****iptables****的常用指令及参数**

Iptalbes 是用来设置、维护和检查Linux内核的IP包过滤规则的。

可以定义不同的表，每个表都包含几个内部的链，也能包含用户定义的链。每个链都是一个规则列表，对对应的包进行匹配：每条规则指定应当如何处理与之相匹配的包。这被称作'target'(目标)，也可以跳向同一个表内的用户定义的链。

**TARGETS**

防火墙的规则指定所检查包的特征，和目标。如果包不匹配，将送往该链中下一条规则检查;如果匹配,那么下一条规则由目标值确定.该目标值可以是用户 定义的链名,或是某个专用值,如ACCEPT[通过], DROP[删除], QUEUE[排队], 或者 RETURN[返回]。

ACCEPT 表示让这个包通过。DROP表示将这个包丢弃。QUEUE表示把这个包传递到用户空间。RETURN表示停止这条链的匹配，到前一个链的规则重新开始。如 果到达了一个内建的链(的末端)，或者遇到内建链的规则是RETURN，包的命运将由链准则指定的目标决定。

**TABLES**

当前有三个表(哪个表是当前表取决于内核配置选项和当前模块)。

-t table

这个选项指定命令要操作的匹配包的表。如果内核被配置为自动加载模块，这时若模块没有加载，(系统)将尝试(为该表)加载适合的模块。这些表如 下：filter,这是默认的表，包含了内建的链INPUT(处理进入的包)、FORWORD(处理通过的包)和OUTPUT(处理本地生成的包)。 nat,这个表被查询时表示遇到了产生新的连接的包,由三个内建的链构成：PREROUTING (修改到来的包)、OUTPUT(修改路由之前本地的包)、POSTROUTING(修改准备出去的包)。mangle 这个表用来对指定的包进行修改。它有两个内建规则：PREROUTING(修改路由之前进入的包)和OUTPUT(修改路由之前本地的包)。

**OPTIONS**

这些可被iptables识别的选项可以区分不同的种类。

**COMMANDS**

这些选项指定执行明确的动作：若指令行下没有其他规定,该行只能指定一个选项.对于长格式的命令和选项名,所用字母长度只要保证iptables能从其他选项中区分出该指令就行了。

-A -append

在所选择的链末添加一条或更多规则。当源(地址)或者/与 目的(地址)转换为多个地址时，这条规则会加到所有可能的地址(组合)后面。

-D -delete

从所选链中删除一条或更多规则。这条命令可以有两种方法：可以把被删除规则指定为链中的序号(第一条序号为1),或者指定为要匹配的规则。

-R -replace

从选中的链中取代一条规则。如果源(地址)或者/与 目的(地址)被转换为多地址，该命令会失败。规则序号从1开始。

-I -insert

根据给出的规则序号向所选链中插入一条或更多规则。所以，如果规则序号为1，规则会被插入链的头部。这也是不指定规则序号时的默认方式。

-L -list

显示所选链的所有规则。如果没有选择链，所有链将被显示。也可以和z选项一起使用，这时链会被自动列出和归零。精确输出受其它所给参数影响。

-F -flush

清空所选链。这等于把所有规则一个个的删除。

--Z -zero

把所有链的包及字节的计数器清空。它可以和 -L配合使用，在清空前察看计数器，请参见前文。

-N -new-chain

根据给出的名称建立一个新的用户定义链。这必须保证没有同名的链存在。

-X -delete-chain

删除指定的用户自定义链。这个链必须没有被引用，如果被引用，在删除之前你必须删除或者替换与之有关的规则。如果没有给出参数，这条命令将试着删除每个非内建的链。

-P -policy

设置链的目标规则。

-E -rename-chain

根据用户给出的名字对指定链进行重命名，这仅仅是修饰，对整个表的结构没有影响。TARGETS参数给出一个合法的目标。只有非用户自定义链可以使用规则，而且内建链和用户自定义链都不能是规则的目标。

-h Help.

[root@pooy~]# iptables [-AI 链名] [-io 网路介面] [-p 协定] [-s 来源IP/网域] [-d 目标IP/网域] -j [ACCEPT|DROP|REJECT|LOG]

选项与参数：
-AI 链名：针对某的链进行规则的 "插入" 或 "累加"
-A ：新增加一条规则，该规则增加在原本规则的最后面。例如原本已经有四条规则，
使用 -A 就可以加上第五条规则！
-I ：插入一条规则。如果没有指定此规则的顺序，预设是插入变成第一条规则。
例如原本有四条规则，使用 -I 则该规则变成第一条，而原本四条变成 2~5 号
链 ：有 INPUT, OUTPUT, FORWARD 等，此链名称又与 -io 有关，请看底下。

-io 网路介面：设定封包进出的介面规范
-i ：封包所进入的那个网路介面，例如 eth0, lo 等介面。需与 INPUT 链配合；
-o ：封包所传出的那个网路介面，需与 OUTPUT 链配合；

-p 协定：设定此规则适用於哪种封包格式
主要的封包格式有： tcp, udp, icmp 及 all 。

-s 来源 IP/网域：设定此规则之封包的来源项目，可指定单纯的 IP 或包括网域，例如：
IP ：192.168.0.100
网域：192.168.0.0/24, 192.168.0.0/255.255.255.0 均可。
若规范為『不许』时，则加上 ! 即可，例如：
-s ! 192.168.100.0/24 表示不许 192.168.100.0/24 之封包来源；

-d 目标 IP/网域：同 -s ，只不过这裡指的是目标的 IP 或网域。

-j ：后面接动作，主要的动作有接受(ACCEPT)、丢弃(DROP)、拒绝(REJECT)及记录(LOG)

--sport：源端口

--dport：目的端口

**看到这里可能觉得很迷糊吧，呵呵,没关系上面的,知道有那么个东西就够了，重点在下面：**

iptables的三种默认规则（指定链）

IUPUT OUTPUT FORWARD

#iptables -P INPUT DROP

#iptables -P OUTPUT DROP

#iptables -P FORWORD DROP

＃将iptables 的 INPUT 、OUTPUT、FORWORD链的默认规则由ACCEPT变为DROP，即关闭所有端口 #（现在还没开始呢）

看到这里还是觉得不知道这个是干嘛的，可以看看iptables的原理

**第二，实际操作****iptables**

来两句试试：

添加一条 允许IP为218.240.27.242 通过22端口访问

iptables -A INPUT  -p tcp -s 218.240.27.242 --dport 22 -j ACCEPT

删除一条

iptables -D INPUT -p tcp -s 218.240.27.242 --dport 22 -j ACCEPT

查看iptables配置情况：iptables -L -n

#----------------配置开始----------------------

linux下IPTABLES配置详解
如果你的IPTABLES基础知识还不了解,建议先去看看.
开始配置
我们来配置一个filter表的防火墙.
(1)查看本机关于IPTABLES的设置情况
[root@pooy ~]# iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
Chain RH-Firewall-1-INPUT (0 references)
target     prot opt source               destination
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0           icmp type 255
ACCEPT     esp  --  0.0.0.0/0            0.0.0.0/0
ACCEPT     ah   --  0.0.0.0/0            0.0.0.0/0
ACCEPT     udp  --  0.0.0.0/0            224.0.0.251         udp dpt:5353
ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0           udp dpt:631
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:22
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:80
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:25
REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited
可以看出我在安装linux时,选择了有防火墙,并且开放了22,80,25端口.
如果你在安装linux时没有选择启动防火墙,是这样的
[root@pooy ~]# iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
什么规则都没有.

(2)清除原有规则.
不管你在安装linux时是否启动了防火墙,如果你想配置属于自己的防火墙,那就清除现在filter的所有规则.
[root@pooy ~]# iptables -F      清除预设表filter中的所有规则链的规则
[root@pooy ~]# iptables -X      清除预设表filter中使用者自定链中的规则
我们在来看一下
[root@pooy ~]# iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
什么都没有了吧,和我们在安装linux时没有启动防火墙是一样的.(提前说一句,这些配置就像用命令配置IP一样,重起就会失去作用),怎么保存？

先将防火墙规则保存到/etc/iptables.up.rules文件中

<pre>[root@pooy ~]# iptables-save &gt; /etc/iptables.up.rules</pre>
<pre></pre>
<pre>然后修改脚本/etc/network/interfaces，使系统能自动应用这些规则（最后一行是我们手工添加的）。</pre>
<pre>[root@pooy ~]#vi /etc/network/interfaces</pre>
<pre>auto eth0</pre>
<pre>iface eth0 inet dhcp</pre>
<pre>pre-up iptables-restore &lt; /etc/iptables.up.rules</pre>
<pre></pre>
<pre>当网络接口关闭后，您可以让iptables使用一套不同的规则集。</pre>
<pre>auto eth0</pre>
<pre>iface eth0 inet dhcp</pre>
<pre>pre-up iptables-restore &lt; /etc/iptables.up.rules</pre>
<pre>post-down iptables-restore &lt; /etc/iptables.down.rules</pre>
<pre></pre>
<pre></pre>
<pre>大多数人并不需要经常改变他们的防火墙规则，因此只要根据前面的介绍，建立起防火墙规则就可以了。但是如果您要经常修改防火墙规则，以使其更加完善，那么 您可能希望系统在每次重启前将防火墙的设置保存下来。为此您可以在/etc/network/interfaces文件中添加一行：</pre>
<pre>[root@pooy ~]#vi /etc/network/interfaces</pre>
<pre>pre-up iptables-restore &lt; /etc/iptables.up.rules</pre>
<pre>post-down iptables-save &gt; /etc/iptables.up.rules</pre>
<pre></pre>
<pre>&quot;post-down iptables-save &gt; /etc/iptables.up.rules&quot;会将设置保存下来，以便下次启动时使用。</pre>
<pre></pre>
<pre>使用iptables-save和iptables-restore可以很方便地修改和测试防火墙规则。首先运行iptables-save将规则保存到一个文件，然后用编辑器编辑该文件。</pre>
<pre>[root@pooy ~]#iptables-save &gt; /etc/iptables.test.rules</pre>
<pre>[root@pooy ~]#vi /etc/iptables.test.rules</pre>
<pre></pre>
<pre>如果您根据前面的例子建立了防火墙规则，iptables-save将产生一个类似于如下内容的文件：</pre>
<pre>[root@pooy ~]#vi /etc/iptables.test.rules</pre>
<pre># Generated by iptables-save v1.3.1 on Sun Apr 23 06:19:53 2006</pre>
<pre>*filter</pre>
<pre>:INPUT ACCEPT [368:102354]</pre>
<pre>:FORWARD ACCEPT [0:0]</pre>
<pre>:OUTPUT ACCEPT [92952:20764374]</pre>
<pre>-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT</pre>
<pre>-A INPUT -i eth0 -p tcp -m tcp --dport 22 -j ACCEPT</pre>
<pre>-A INPUT -i eth0 -p tcp -m tcp --dport 80 -j ACCEPT</pre>
<pre>-A INPUT -i lo -j ACCEPT</pre>
<pre>-A INPUT -m limit --limit 5/min -j LOG --log-prefix &quot;iptables denied: &quot; --log-level 7</pre>
<pre>-A INPUT -j DROP</pre>
<pre>COMMIT</pre>
<pre># Completed on Sun Apr 23 06:19:53 2006</pre>
<pre></pre>
<pre>文件内容其实就是各种iptables命令，只不过把命令名iptables省略了。您可以随意对这个文件进行编辑，然后保存。接着使用以下命令测试修改后的规则：</pre>
<pre></pre>
<pre>[root@pooy ~]#iptables-restore &lt; /etc/iptables.test.rules</pre>
<pre></pre>
<pre>之前您如果没有在/etc/network/interfaces
文件中添加iptables-save
命令，那么测试之后，别忘了把您所作的修改保存起来。</pre>
<pre>[root@pooy ~]#iptables-save &gt; /etc/iptables.up.rules</pre>
<pre></pre>
<pre>您可以创建额外的规则链，以便在syslog中作更加详细的记录。以下是我/etc/iptables.up.rules文件中的一个简单例子：</pre>
<pre># Generated by iptables-save v1.3.1 on Sun Apr 23 05:32:09 2006</pre>
<pre>*filter</pre>
<pre>:INPUT ACCEPT [273:55355]</pre>
<pre>:FORWARD ACCEPT [0:0]</pre>
<pre>:LOGNDROP - [0:0]</pre>
<pre>:OUTPUT ACCEPT [92376:20668252]</pre>
<pre>-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT</pre>
<pre>-A INPUT -i eth0 -p tcp -m tcp --dport 22 -j ACCEPT</pre>
<pre>-A INPUT -i eth0 -p tcp -m tcp --dport 80 -j ACCEPT</pre>
<pre>-A INPUT -i lo -j ACCEPT</pre>
<pre>-A INPUT -j LOGNDROP</pre>
<pre>-A LOGNDROP -p tcp -m limit --limit 5/min -j LOG --log-prefix &quot;Denied TCP: &quot; --log-level 7</pre>
<pre>-A LOGNDROP -p udp -m limit --limit 5/min -j LOG --log-prefix &quot;Denied UDP: &quot; --log-level 7</pre>
<pre>-A LOGNDROP -p icmp -m limit --limit 5/min -j LOG --log-prefix &quot;Denied ICMP: &quot; --log-level 7</pre>
<pre>-A LOGNDROP -j DROP</pre>
<pre>COMMIT</pre>
<pre># Completed on Sun Apr 23 05:32:09 2006</pre>
<pre></pre>
<pre>可以通过清除所有规则来暂时停止防火墙： (警告：这只适合在没有配置防火墙的环境中，如果已经配置过默认规则为deny的环境，此步骤将使系统的所有网络访问中断)</pre>
<pre>[root@pooy ~]# sudo iptables -F</pre>
<pre></pre>
<pre>是不是保存上了呢？可以reboot一下试试。</pre>
<pre></pre>
<pre>(3)设定预设规则</pre>
<pre>[root@pooy ~]# iptables -P INPUT DROP</pre>
[root@pooy ~]# iptables -P OUTPUT ACCEPT
[root@pooy ~]# iptables -P FORWARD DROP
上面的意思是,当超出了IPTABLES里filter表里的两个链规则(INPUT,FORWARD)时,不在这两个规则里的数据包怎么处理呢,那就是DROP(放弃).应该说这样配置是很安全的.我们要控制流入数据包
而对于OUTPUT链,也就是流出的包我们不用做太多限制,而是采取ACCEPT,也就是说,不在着个规则里的包怎么办呢,那就是通过.
可以看出INPUT,FORWARD两个链采用的是允许什么包通过,而OUTPUT链采用的是不允许什么包通过.
这样设置还是挺合理的,当然你也可以三个链都DROP,但这样做我认为是没有必要的,而且要写的规则就会增加.但如果你只想要有限的几个规则是,如只做WEB服务器.还是推荐三个链都是DROP.
注:如果你是远程SSH登陆的话,当你输入第一个命令回车的时候就应该掉了.因为你没有设置任何规则.
怎么办,去本机操作呗!
(4)添加规则.
首先添加INPUT链,INPUT链的默认规则是DROP,所以我们就写需要ACCETP(通过)的链。
为了能采用远程SSH登陆,我们要开启22端口.
[root@pooy ~]# iptables -A INPUT -p tcp --dport 22 -j ACCEPT
[root@pooy ~]# iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT (注:这个规则,如果你把OUTPUT 设置成DROP的就要写上这一部,好多人都是望了写这一部规则导致,始终无法SSH.在远程一下,是不是好了.
其他的端口也一样,如果开启了web服务器,OUTPUT设置成DROP的话,同样也要添加一条链:
[root@pooy ~]# iptables -A OUTPUT -p tcp --sport 80 -j ACCEPT ,其他同理.)
如果做了WEB服务器,开启80端口.
[root@pooy ~]# iptables -A INPUT -p tcp --dport 80 -j ACCEPT
如果做了邮件服务器,开启25,110端口.
[root@pooy ~]# iptables -A INPUT -p tcp --dport 110 -j ACCEPT
[root@pooy ~]# iptables -A INPUT -p tcp --dport 25 -j ACCEPT
如果做了FTP服务器,开启21端口
[root@pooy ~]# iptables -A INPUT -p tcp --dport 21 -j ACCEPT
[root@pooy ~]# iptables -A INPUT -p tcp --dport 21 -j ACCEPT
如果做了DNS服务器,开启53端口
[root@pooy ~]# iptables -A INPUT -p tcp --dport 53 -j ACCEPT
如果你还做了其他的服务器,需要开启哪个端口,照写就行了.
上面主要写的都是INPUT链,凡是不在上面的规则里的,都DROP
允许icmp包通过,也就是允许ping,
[root@pooy ~]# iptables -A OUTPUT -p icmp -j ACCEPT (OUTPUT设置成DROP的话)
[root@pooy ~]# iptables -A INPUT -p icmp -j ACCEPT  (INPUT设置成DROP的话)
允许loopback!(不然会导致DNS无法正常关闭等问题)
IPTABLES -A INPUT -i lo -p all -j ACCEPT (如果是INPUT DROP)
IPTABLES -A OUTPUT -o lo -p all -j ACCEPT(如果是OUTPUT DROP)
下面写OUTPUT链,OUTPUT链默认规则是ACCEPT,所以我们就写需要DROP(放弃)的链.
减少不安全的端口连接
[root@pooy ~]# iptables -A OUTPUT -p tcp --sport 31337 -j DROP
[root@pooy ~]# iptables -A OUTPUT -p tcp --dport 31337 -j DROP
有些些特洛伊木马会扫描端口31337到31340(即黑客语言中的 elite  [mltechs.com](http://4seohunt.biz/rep/mltechs.com) 端口)上的服务。既然合法服务都不使用这些非标准端口来通信,阻塞这些端口能够有效地减少你的网络上可能被感染的机器和它们的远程主服务器进行独立通信的机会
还有其他端口也一样,像:31335、27444、27665、20034  NetBus、9704、137-139（smb）,2049(NFS)端口也应被禁止,我在这写的也不全,有兴趣的朋友应该去查一下相关资料.

当然出入更安全的考虑你也可以包OUTPUT链设置成DROP,那你添加的规则就多一些,就像上边添加
允许SSH登陆一样.照着写就行了.

下面写一下更加细致的规则,就是限制到某台机器
如:我们只允许192.168.0.3的机器进行SSH连接
[root@pooy ~]# iptables -A INPUT -s 192.168.0.3 -p tcp --dport 22 -j ACCEPT
如果要允许,或限制一段IP地址可用 192.168.0.0/24 表示192.168.0.1-255端的所有IP.
24表示子网掩码数.但要记得把 /etc/sysconfig/iptables 里的这一行删了.
-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT 因为它表示所有地址都可以登陆.
或采用命令方式:
[root@pooy ~]# iptables -D INPUT -p tcp --dport 22 -j ACCEPT

这样写 !192.168.0.3 表示除了192.168.0.3的ip地址
其他的规则连接也一样这么设置.

在下面就是FORWARD链,FORWARD链的默认规则是DROP,所以我们就写需要ACCETP(通过)的链,对正在转发链的监控.
开启转发功能,(在做NAT时,FORWARD默认规则是DROP时,必须做)
[root@pooy ~]# iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
[root@pooy ~]# iptables -A FORWARD -i eth1 -o eh0 -j ACCEPT
丢弃坏的TCP包
[root@pooy ~]#iptables -A FORWARD -p TCP ! --syn -m state --state NEW -j DROP
处理IP碎片数量,防止攻击,允许每秒100个
[root@pooy ~]#iptables -A FORWARD -f -m limit --limit 100/s --limit-burst 100 -j ACCEPT
设置ICMP包过滤,允许每秒1个包,限制触发条件是10个包.
[root@pooy ~]#iptables -A FORWARD -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT
我在前面只所以允许ICMP包通过,就是因为我在这里有限制.
二,配置一个NAT表放火墙
1,查看本机关于NAT的设置情况
[root@tp rc.d]# iptables -t nat -L
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination
Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination
SNAT       all  --  192.168.0.0/24       anywhere            to:211.101.46.235
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
我的NAT已经配置好了的(只是提供最简单的代理上网功能,还没有添加防火墙规则).关于怎么配置NAT,参考我的另一篇文章
当然你如果还没有配置NAT的话,你也不用清除规则,因为NAT在默认情况下是什么都没有的
如果你想清除,命令是
[root@pooy ~]# iptables -F -t nat
[root@pooy ~]# iptables -X -t nat
[root@pooy ~]# iptables -Z -t nat

2,添加规则（不懂路由规则的可以跳过）
例：
禁止与211.101.46.253的所有连接
[root@pooy ~]# iptables -t nat -A PREROUTING  -d 211.101.46.253 -j DROP
禁用FTP(21)端口
[root@pooy ~]# iptables -t nat -A PREROUTING -p tcp --dport 21 -j DROP
这样写范围太大了,我们可以更精确的定义.
[root@pooy ~]# iptables -t nat -A PREROUTING  -p tcp --dport 21 -d 211.101.46.253 -j DROP
这样只禁用211.101.46.253地址的FTP连接,其他连接还可以.如web(80端口)连接.
按照我写的,你只要找到QQ,MSN等其他软件的IP地址,和端口,以及基于什么协议,只要照着写就行了.

最后：
drop非法连接
[root@pooy ~]# iptables -A INPUT   -m state --state INVALID -j DROP
[root@pooy ~]# iptables -A OUTPUT  -m state --state INVALID -j DROP
[root@pooy ~]# iptables-A FORWARD -m state --state INVALID -j DROP
允许所有已经建立的和相关的连接
[root@pooy ~]# iptables-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
[root@pooy ~]# iptables-A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
<pre>前面我们把iptables的规则写到了/etc/network/interfaces来做保存，系统如果突然重启的话也没事，而且这个是实时生效的。我们刚才写的这些也不会丢失！到这里iptables的文档就结束了。希望能对你有所帮助！</pre>
<pre>欢迎访问：&lt;a href=&quot;http://www.pooy.net/&quot;&gt;www.pooy.net&lt;/a&gt; 璞玉的博客！</pre>