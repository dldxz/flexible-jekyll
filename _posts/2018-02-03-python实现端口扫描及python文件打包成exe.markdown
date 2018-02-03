---
layout: post
title: python实现端口扫描及python文件打包成exe
date: 2018-02-03 15:41:20 -0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: desktop.jpg # Add image post (optional)
tags: [python，端口扫描，pyinstaller]
---
时隔两天，又回来了。

话说这个端口扫描其实昨天就已经做好了，但是由于各种各样的原因没能发出来(才不是因为追番而忘记发了呢，手动滑稽）

先说一下端口扫描的思路吧，一开始我以为很简单的调用windows的shell，ping一下就完全ojbk了，实际上等我真正ping了之后才发现ping只能ping ip，ping不了端口(╯‵□′)╯︵┻━┻

然后又查了一下，发现还是可以通过windows的shell来扫描，但是要下载一个工具，对于习惯了python的简洁的我来说这简直就是要老命，遂放弃。

最终找到了一个之前在c语言了碰见过的东西：*socket*,老实说现在我也不会这东西，只能算是对着文档勉强依样画葫芦画了一个出来。想当初在某逼乎上看到某dalao说 *socket*算是大二学生的基本操作的我是有多么的绝望啊(lll￢ω￢)

回到代码上，*socket*模块里要用到的函数有两个，*socket.socket*和 *socket.connect*，
先把官方文档的描述写上：

>
socket.socket(family=AF_INET, type=SOCK_STREAM, proto=0, fileno=None)
Create a new socket using the given address family, socket type and protocol number. The address family should be AF_INET (the default), AF_INET6, AF_UNIX, AF_CAN or AF_RDS. The socket type should be SOCK_STREAM (the default), SOCK_DGRAM, SOCK_RAW or perhaps one of the other SOCK_ constants. The protocol number is usually zero and may be omitted or in the case where the address family is AF_CAN the protocol should be one of CAN_RAW or CAN_BCM. If fileno is specified, the other arguments are ignored, causing the socket with the specified file descriptor to return. Unlike socket.fromfd(), fileno will return the same socket and not a duplicate. This may help close a detached socket using socket.close().
>
socket.connect(address)
Connect to a remote socket at address. (The format of address depends on the address family — see above.)
>
If the connection is interrupted by a signal, the method waits until the connection completes, or raise a socket.timeout on timeout, if the signal handler doesn’t raise an exception and the socket is blocking or has a timeout. For non-blocking sockets, the method raises an InterruptedError exception if the connection is interrupted by a signal (or the exception raised by the signal handler).
{% endhighlight %}


第一个函数用来生成一个套接字，emmmm，我也不是很懂这个套接字是个什么意思，总之要这样用就对了，第二个函数就是用来连接我们需要连接的端口了，注意看描述，它会等待至连接完成或者是超时，我估计最后的程序运行起来速度很慢就是这个原因，等什么时候看看能不能改进下。

代码的话，和zip爆破差不多，都是进循环然后 *try*.
代码如下：

{% highlight python %}
import socket
ip=input("请输入要扫描的ip地址：")
PortBegin=input("请输入开始扫描的端口：")
PortEnd=input("请输入结束扫描的端口：")
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
for i in range(int(PortBegin),int(PortEnd)+1):
    try:
        a=s.connect((ip,i))
        pass
    except socket.error:
        continue
    else:
        print(i)
{% endhighlight %}

嗯。我写的时候就没有考虑过用户体验所以别说什么不好用（逃


哦对差点忘了 *pyinstaller*了，这是一个可以把py文件打包成exe程序的东西？
用pip下载，cmd输入pip install pyinstaller安装 *pyinstaller*，之后这个程序应该会在你的python/script文件夹里。

cmd输入pyinstaller.exe -F py文件路径，之后就是等待了。
话说我在打包端口扫描的时候总是报错
![](‪{{site.baseurl}}\assets\img\UnicodeDecodeError.jpg)

找遍了所有的博客，终于看到了一个和我抱着一样问题的仁兄，下面有一位前辈说在cmd输入 *chcp 65001*，试了一下还是一样的报错，不过前辈终究是前辈，另一个方法是不要放在非英文目录下，emmmm，这样的错误我应该是不会犯的，马萨卡

好吧我的文件名是全中文的(╯‵□′)╯︵┻━┻，zip爆破也有中文啊为什么它这么顺利
![]({{site.baseurl}}\assets\img\succss.jpg)
![]({{site.baseurl}}\assets\img\exefile.jpg)

总之最后还是成功了，okey