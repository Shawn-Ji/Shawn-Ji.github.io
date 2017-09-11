---
layout:     post
title:      端口转发配置记录
cover: "https://ws1.sinaimg.cn/large/db141f43gy1fjfwlv4xq8j222x109qhd.jpg"
tags: 端口转发
---

### 记录下Linux、Windows下的端口转发
#### Linux
这个在之前配置openvpn的时候已经做过贴上[链接](https://shawn-ji.github.io/2017/06/02/openvpn.html#section-3-9)
在这里再简单介绍一下常用命令
```bash
这里将10.8.0.1:80 转发到10.8.0.6:80
//开启转发功能
sysctl net.ipv4.ip_forward=1
//添加
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to 10.8.0.6:80
iptables -t nat -A POSTROUTING -d 10.8.0.6 -p tcp --dport 80 -j SNAT --to 10.8.0.1
//删除
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to 10.8.0.6:80
iptables -t nat -A POSTROUTING -d 10.8.0.6 -p tcp --dport 80 -j SNAT --to 10.8.0.1
//查看
iptables -t nat -L
```

#### Windows
Windows上和Linux一样,也有类似的命令netsh

需要有管理员权限，防火墙需要开放指定的端口

很简单的命令
```bash
//添加
netsh interface portproxy add v4tov4 listenport=80 connectaddress=172.16.25.3 connectport=80
//删除
netsh interface portproxy delete v4tov4 listenport=80
//查看所有
netsh interface portproxy show all
```
