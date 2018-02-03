---
layout: post
title: python实现zip密码爆破
date: 2018-02-01 08:09:20 -0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: software.jpg # Add image post (optional)
tags: [python，爆破]
---
emmmm
寒假闲得蛋疼，正好上学期学校协会有几个和自己有关的项目，今天找了一个用python实现zip密码爆破的实现了一下
暂时只是初步的代码实现，至于后面的GUI以及打包成程序打算过几天再说（老家冷的我敲代码手都僵的）

首先思路，要实现爆破，并且是自带字典的爆破，先要能打开zip文件
在python官方文档里，有一个叫 *zipfile*的模块，里面有很多操作zip文件的函数，这里就不多赘述

我们要用的是里面的ZipFile类里面的extractall函数
这是官方文档中对于 *extractall*函数的说明
>
ZipFile.extractall(path=None, members=None, pwd=None)
Extract all members from the archive to the current working directory. path specifies a different directory to extract to.
members is optional and must be a subset of the list returned by namelist(). pwd is the password used for encrypted files.

第一个参数是解压的路径，如果要解压到与zip文件不同的路径的话可以填上，这里直接解压到当前目录所以我没写
第二个参数是zip文件中需要解压的文件？不是很清楚，不过这里我也没用这个参数所以略过
第三个参数就是我们这个密码爆破的核心了，密码。

再来我们要打开字典文件，python内置函数有一个 *open()*函数
这是runoob网站上对于 *open()*函数的说明
>
open(name, mode, buffering)
name : 一个包含了你要访问的文件名称的字符串值。
mode : mode 决定了打开文件的模式：只读，写入，追加等。所有可取值见如下的完全列表。这个参数是非强制的，默认文件访问模式为只读(r)。
buffering : 如果 buffering 的值被设为 0，就不会有寄存。如果 buffering 的值取 1，访问文件时会寄存行。如果将 buffering 的值设为大于 1 的整数，表明了这就是的寄存区的缓冲大小。如果取负值，寄存区的缓冲大小则为系统默认。

对于字典文件我们只需要只读，第三个参数可以无视

主体有了，加个循环再加个 *try*，代码就算是写好了

{% highlight python %}
    import zipfile
    print("使用时请将程序放至密码文件与zip文件同一文件夹！\n")
    dictlocation=input("请输入字典名字：")
    ziplocation=input("请输入zip文件：")
    a=open(dictlocation,"r")
    f=zipfile.ZipFile(ziplocation)
    for line in a:
        ps=line[:-1]
    	try:
        	y=f.extractall(pwd=ps.encode('utf-8'))
        	print("Found PassWord:",ps)
        	print("解压成功！")
        	break
        except:
        	continue
        else:
    	    print("爆破失败！")
{% endhighlight %}

莫名其妙缩进消失了，容我去研究下markdown
以上是暂时的代码，GUI以及打包程序我会尽快试着实现的。