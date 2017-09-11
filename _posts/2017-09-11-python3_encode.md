---
layout: post
title: 'Python3 中的字符编码问题'
date: 2017-09-11
cover: 'https://ws1.sinaimg.cn/large/db141f43gy1fjfymvabp9j23ww1gdb2a.jpg'
tags: Python
---

以前就学习过和使用过一段时间的Python , 算是速成吧 , 看的是实验楼上的教程文档 , 把Python快速过的一遍 , 以最快的时间上手了 . 

今天看到廖雪峰的网站上的Python教程 , 觉得质量应该比较高 , 所以准备再看一遍 巩固下.

看到他说的[字符编码问题](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431664106267f12e9bef7ee14cf6a8776a479bdec9b9000) ,  感觉自己以前并没有遇到 , 但是我不想就这么放过这个**暂时没遇到过**的问题 .

文章中提到一个概念 , 如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes . 

从没有接触过这个`bytes` 类型的数据 . 

所以便找个了例子:

从redis里读取一个字符串

```python
import redis
 from urllib.parse import urlparse
 key = 'baidu'
val = 'http://www.baidu.com'
schemes = ['http', 'https', 'socks']
rdb = redis.StrictRedis(host='localhost', port=6379, db=0)
rdb.set('baidu', 'http://www.baidu.com')
url = rdb.get('baidu')
```

这时候 `url` 这个变量不是不同的str  而是Python3 中的`bytes`
> Python2 中字符串分为 `str` 和 `unicode` 2种
> Python2 中字符串分为 `bytes` 和 `str` 2种
> Python3 种的`bytes` 相当于 Python2中的`str`  一个概念的不同叫法

**以下所有的叫法以Python3为准**

> Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。

所以从redis服务器get到的字符串是`bytes`类型的,  需要encode成`str`类型.

```python
url = url.encode('utf8')
print (url)
```

**写在最后**
> 我个人是比较喜欢 Python3 的更新的，默认的 utf-8 编码解决了很多的问题。

> 相比于 Python2，可能 Python3 的处理要繁琐一点，但安全性会好很多，一些很奇怪的问题可以及时发现。例如 decode 和 encode 方法的明确。同时，因为这些变化，我们需要在 bytes 可能出现的地方留心（一般是程序外部来的数据），进行类型转换，数据交互的层面统一使用 str 进行处理。

> 与 Python2 相比，str 和 bytes 的命名其实也更贴近实际的情况。我是这样去记两者的关系的：str 是 unicode 的 code 的序列，可认为是该字符在世界的唯一标识（code point），而 bytes 则是 str 通过某种编码（utf-8）实际保存的二进制数据。unicode 是种协议，而 utf-8 是这种协议的某种实现方式。

参考:
[lycheng's blog](https://lycheng.github.io/2017/05/17/python3-str-bytes.html)