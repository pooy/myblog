---
title: 'Apache的Order Allow,Deny 详解'
tags:
  - Apache的Deny 详解，apache
  - Apache的Order详解，Apache的Allow详解
id: 1072
categories:
  - Linux
  - 手册资料
date: 2013-02-25 17:29:45
---

Apache的Order Allow,Deny 详解
Allow和Deny可以用于apache的conf文件或者.htaccess文件中（配合Directory, Location, Files等），用来控制目录和文件的访问授权。

所以，最常用的是：
Order Deny,Allow
Allow from All

注意“Deny,Allow”中间只有一个逗号，也只能有一个逗号，有空格都会出错；单词的大小写不限。上面设定的含义是先设定“先检查禁止设定，没有禁止的全部允许”，而第二句没有Deny，也就是没有禁止访问的设定，直接就是允许所有访问了。这个主要是用来确保或者覆盖上级目录的设置，开放所有内容的访问权。

按照上面的解释，下面的设定是无条件禁止访问：
Order Allow,Deny
Deny from All

如果要禁止部分内容的访问，其他的全部开放：
Order Deny,Allow
Deny from ip1 ip2
或者
Order Allow,Deny
Allow from all
Deny from ip1 ip2

apache会按照order决定最后使用哪一条规则，比如上面的第二种方式，虽然第二句allow允许了访问，但由于在order中allow不是最后规则，因此还需要看有没有deny规则，于是到了第三句，符合ip1和ip2的访问就被禁止了。注意，order决定的“最后”规则非常重要，下面是两个错误的例子和改正方式：

Order Deny,Allow
Allow from all
Deny from domain.org
错误：想禁止来自domain.org的访问，但是deny不是最后规则，apache在处理到第二句allow的时候就已经匹配成功，根本就不会去看第三句。
解决方法：Order Allow,Deny，后面两句不动，即可。

Order Allow,Deny
Allow from ip1
Deny from all
错误：想只允许来自ip1的访问，但是，虽然第二句中设定了allow规则，由于order中deny在后，所以会以第三句deny为准，而第三句的范围中又明显包含了ip1（all include ip1），所以所有的访问都被禁止了。
解决方法一：直接去掉第三句。
解决方法二：
Order Deny,Allow
Deny from all
Allow from ip1

下面是测试过的例子：
--------------------------------
Order deny,allow
allow from all
deny from 219.204.253.8
#全部都可以通行
-------------------------------
Order deny,allow
deny from 219.204.253.8
allow from all
#全部都可以通行
-------------------------------
Order allow,deny
deny from 219.204.253.8
allow from all
#只有219.204.253.8不能通行
-------------------------------
Order allow,deny
allow from all
deny from 219.204.253.8
#只有219.204.253.8不能通行
-------------------------------
-------------------------------
Order allow,deny
deny from all
allow from 219.204.253.8
#全部都不能通行
-------------------------------
Order allow,deny
allow from 219.204.253.8
deny from all
#全部都不能通行
-------------------------------
Order deny,allow
allow from 219.204.253.8
deny from all
#只允许219.204.253.8通行
-------------------------------
Order deny,allow
deny from all
allow from 219.204.253.8
#只允许219.204.253.8通行
-------------------------------
--------------------------------
Order deny,allow
#全部都可以通行（默认的）
-------------------------------
Order allow,deny
#全部都不能通行（默认的）
-------------------------------
Order allow,deny
deny from all
#全部都不能通行
-------------------------------
Order deny,allow
deny from all
#全部都不能通行
-------------------------------
对于上面两种情况，如果换成allow from all，则全部都可以通行！
-------------------------------
Order deny,allow
deny from 219.204.253.8
#只有219.204.253.8不能通行
-------------------------------
Order allow,deny
deny from 219.204.253.8
#全部都不能通行
-------------------------------
Order allow,deny
allow from 219.204.253.8
#只允许219.204.253.8通行
-------------------------------
Order deny,allow
allow from 219.204.253.8
#全部都可以通行
-------------------------------
-------------------------------
order deny,allow
allow from 218.20.253.2
deny from 218.20
#代表拒绝218.20开头的IP，但允许218.20.253.2通过；而其它非218.20开头的IP也都允许通过。
-------------------------------
order allow,deny
allow from 218.20.253.2
deny from 218.20
#和上面的差不多，只是掉换的order语句中的allow、deny先后顺序，但最终结果表示全部都拒绝！
form:http://hi.baidu.com/enjoypain/blog/item/f48c7aecdba298d12f2e21ac.html

前段时间做了个Apache的HTTP代理服务器，其中的order allow，deny这部分弄的不太懂，于是上网找资料看，谁知道越看越糊涂，其中有些难以分辨对错甚至是误导。就像破解windows系统密码的一些文章那样，很多都是人云亦云的，并没有经过测试。废话少说，先把我经过测试后分析总结出来的结论show出来，相信这对大家的理解非常有帮助。

总则——

影响最终判断结果的只有两点：

1\. order语句中allow、deny的先后顺序；

2\. allow、deny语句中各自包含的范围。

温馨提醒——

1\. 修改完配置后要保存好并重启Apache服务，配置才能生效；

2\. 开头字母不分大小写；

3\. allow、deny语句不分先后顺序，谁先谁后不影响最终判断结果；但都会被判断到；

4\. order语句中，“allow,deny”之间“有且只有”一个逗号（英文格式的），而且先后顺序很重要；

5\. Apache有一条缺省规则，“order allow,deny”本身就默认了拒绝所有的意思，因为deny在allow的后面；同理，“order deny,allow”本身默认的是允许所有；当然，最终判断结果还要综合下面的allow、deny语句中各自所包含的范围；（也就是说order语句后面可以没有allow、deny语句）

6\. allow、deny语句中，第二个单词一定是“from”，否则Apache会因错而无法启动，

7\. “order allow,deny”代表先判断allow语句再判断deny语句，反之亦然。

上面说的都是要记住的，而下面说的是我独创的理解方法。如果有人看了而没有豁然开朗的感觉，那算是我的失败！

判断原则分4步走——

1\. 首先判断默认的；

2\. 然后判断逗号前的；

3\. 最后判断逗号后的；

4\. 最终按顺序叠加而得出判断结果。

上面三点我说的简单而形象，主要是为了便于记忆。暂时不理解不要紧，继续看下面详细的解说自然会明白。下面以一个普通例子来做解释——

order deny,allow

allow from 218.20.253.2

deny from 218.20

1\. 所谓“首先判断默认的”，就是判断“order deny,allow”这句，它默认是允许所有；

2\. 所谓“然后判断逗号前的”，因为在本例子中的order语句里面，deny在逗号的前面，所以现在轮到判断下面的deny语句了——“deny from 218.20”；

3\. 所谓“最后判断逗号后的”，因为在本例子中的order语句里面，allow在逗号的后面，所以最后轮到判断下面的allow语句了——“allow from 218.20.253.2”。

4\. 所谓“最终按顺序叠加而得出判断结果”，这是一个形象化了的说法，我把每一步判断都看作一个“不透明的图层”，然后一步步按顺序叠加上去，最终得出的“图像”就是判断结果。

用过作图软件的人应该都知道“图层”是怎么回事，我估计Apache关于order allow deny这方面的设计理念和photoshop等作图软件关于图层的设计理念是一样的。即“游戏规则”是一样的。

那么上面的例子就可以是这么一个步骤和图像——

1\. 先画一个白色的大圆，代表“order deny,allow”语句，默认意思是允许所有；

2\. 然后画一个小一点的黑色圆，代表“deny from 218.20”语句，意思是拒绝所有以218.20开头的IP，放进白色的大圆里面；

3\. 最后再画一个白色的圆，代表“allow from 218.20.253.2”语句，意思是允许218.20.253.2通过，放在黑色圆的上面。

4\. 到此为止，我们已经可以看到一个结果了，白色大圆上面有一个黑色圆，黑色圆上面还有一个白色圆。最后，我们所能看到的黑色部分就是拒绝通行的，剩下的白色部分都是允许通行的。判断的结果就是这么简单形象！

如果不懂的用作图软件，我们再来个非常贴近生活的比喻——

把上面的例子改动一点点，以便更好的理解：

order deny,allow

allow from 218.20.253.2

deny from 219.30

1\. 首先拿一张A4白纸，代表第order语句，意思是允许全部；

2\. 然后拿一张黑色纸剪一个圆，放在A4纸里面的某个位置上，代表deny语句，意思是拒绝所有以219.30开头的IP；

3\. 最后拿白纸再剪一个圆，放在黑色圆的旁边，代表allow语句，意思是允许218.20.253.2通过；注意，这个例子不是放进黑色圆里面了，因为deny和allow语句不再有相互包含的关系了。

4\. A4纸上面有一个黑色圆和一个白色圆，结果自然很明显了。不过白色的A4纸上再放一个白色的圆，显然是多余的了，因为大家都是白色的，都代表允许，所以就重复了，可以去掉白色的圆而不会影响判断结果。

如果看到这里还没明白，那一定是我还有什么没说清楚的。那么请好好分析我所做过的测试例子，将在最后列出来。

在这里再啰嗦一下，allow、deny语句后面跟的参数有多种形式，有不同的表达方式，我在网上看到的做法是deny from IP1 IP2 IP3或allow from domain.com等。其它的表达方式大家再找别的资料看吧。我想说的是另一种表达方式：

order deny,allow

allow from IP1 IP2

allow from domain.info

allow from 219.20.55.0/24

deny from all

我没具体验证过这是否对，不过这样是可以正常启动Apache服务的，按道理应该是正确的表达方式。哈哈，像我这样的入门者只能这样了，还希望大家多多指教！

下面是测试过的例子：
<pre class="brush: bash; gutter: true">--------------------------------
Order deny,allow
allow from all
deny from 219.204.253.8
#全部都可以通行
-------------------------------
Order deny,allow
deny from 219.204.253.8
allow from all
#全部都可以通行
-------------------------------
Order allow,deny
deny from 219.204.253.8
allow from all
#只有219.204.253.8不能通行
-------------------------------
Order allow,deny
allow from all
deny from 219.204.253.8
#只有219.204.253.8不能通行
-------------------------------
-------------------------------
Order allow,deny
deny from all
allow from 219.204.253.8
#全部都不能通行
-------------------------------
Order allow,deny
allow from 219.204.253.8
deny from all
#全部都不能通行
-------------------------------
Order deny,allow
allow from 219.204.253.8
deny from all
#只允许219.204.253.8通行
-------------------------------
Order deny,allow
deny from all
allow from 219.204.253.8
#只允许219.204.253.8通行
-------------------------------
--------------------------------
Order deny,allow
#全部都可以通行（默认的）
-------------------------------
Order allow,deny
#全部都不能通行（默认的）
-------------------------------
Order allow,deny
deny from all
#全部都不能通行
-------------------------------
Order deny,allow
deny from all
#全部都不能通行
-------------------------------
对于上面两种情况，如果换成allow from all，则全部都可以通行！
-------------------------------
Order deny,allow
deny from 219.204.253.8
#只有219.204.253.8不能通行
-------------------------------
Order allow,deny
deny from 219.204.253.8
#全部都不能通行
-------------------------------
Order allow,deny
allow from 219.204.253.8
#只允许219.204.253.8通行
-------------------------------
Order deny,allow
allow from 219.204.253.8
#全部都可以通行
-------------------------------
-------------------------------
order deny,allow
allow from 218.20.253.2
deny from 218.20
#代表拒绝218.20开头的IP，但允许218.20.253.2通过；而其它非218.20开头的IP也都允许通过。
-------------------------------
order allow,deny
allow from 218.20.253.2
deny from 218.20
#和上面的差不多，只是掉换的order语句中的allow、deny先后顺序，但最终结果表示全部都拒绝！</pre>
以上就是Apache的Order Allow,Deny 详解，有任何问题可以一起讨论。