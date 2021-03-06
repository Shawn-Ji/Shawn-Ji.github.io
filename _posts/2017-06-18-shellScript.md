---
layout:     post
title:      Shell脚本管理多台服务器进程
cover: 'https://ws1.sinaimg.cn/large/db141f43gy1fjfyh9molij24bb1qpkjm.jpg'
tags: Shell脚本
---
# 使用shell脚本对两台Linux服务器进行服务进程管理(1台主服务器，1台从服务器)

> 支持如下功能:
1. 测某个服务器进程是否运行，被监测进程退出后自动重启
2. 主动停止被监测进程，从某台服务器上更新该程序或相关的配置文件，重启该进程

## 对以上两个功能的实现步骤分两步

1. 在主和从服务器上分别建立程序文件和脚本文件，并分别首次启动服务程序和监测服
务程序的脚本
主: helloworld.c、a.out、start.sh、monitor.sh、update.sh
从: helloworld.c、a.out、start.sh、monitor.sh
2. 主动停止主和从服务器的被监测进程和监测进程，并对主和从服务器更新该服务程序，再自动重启被监测进程和监测进程


## Descript：

被监测的程序为a.out,其是helloworld.c的可执行程序


![](https://ooo.0o0.ooo/2017/06/18/5946a1989d196.jpg)


## 具体细节描述以图片的形式展示管理服务进程功能的实现

### 主服务器下的程序文件和脚本文件

![](https://ooo.0o0.ooo/2017/06/18/59468397b239e.png)
### 从服务器下的程序文件和脚本文件

![](https://ooo.0o0.ooo/2017/06/18/59468397b239e.png)

### 启动主服务器该服务程序和监测服务程序的脚本

启动前：

![](https://ooo.0o0.ooo/2017/06/18/5946848c18680.png)

启动后出现pid和进程名:

![](https://ooo.0o0.ooo/2017/06/18/594684b9e9e27.png)


### 启动从服务器该服务程序和监测服务程序的脚本

启动前:

![](https://ooo.0o0.ooo/2017/06/18/594685574025d.png)

启动后出现pid和进程名:

![](https://ooo.0o0.ooo/2017/06/18/5946857c17e24.png)


### 被监测进程挂掉自动重启

杀掉主服务器进程a.out并自动重启,pid发生改变:

![](https://ooo.0o0.ooo/2017/06/18/5946861450cb8.png)

杀掉从服务器进程a.out并自动重启,pid发生改变:

![](https://ooo.0o0.ooo/2017/06/18/5946866bad2bf.png)

### 主动停止被监测进程和监测进程，更新该服务程序并自动重启监测与被监测进程

更新主服务器上的helloworld.c文件

![](https://ooo.0o0.ooo/2017/06/18/594686de40d32.png)

此时从服务器上的helloworld.c还未更新

![](https://ooo.0o0.ooo/2017/06/18/594687b24fe44.png)

#### 在主服务器上运行update.sh脚本，首先停止主和从服务器的监测和被监测进程,将helloworld.c更新到从服务器上，并自动重启监测与被监测进程。

主服务器的被监测与监测进程的pid发生改变

![](https://ooo.0o0.ooo/2017/06/18/594688d1a4118.png)


从服务器上helloworld.c更新成功

![](https://ooo.0o0.ooo/2017/06/18/594689803b8a7.png)

从服务器的被监测与监测进程的pid发生改变

![](https://ooo.0o0.ooo/2017/06/18/59468ab7297c5.png)