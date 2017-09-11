---
layout:     post
title:      Redis实现分布式任务队列
cover: 'https://ws1.sinaimg.cn/large/db141f43gy1fjfyjtgd98j234u1bd7wi.jpg'
tags: redis
---
# Redis实现分布式任务队列
### 核心做法:
1. `setnx`设置分布式锁 `expire`设置锁生存周期 , 防止死锁 . 
2. sort set  `zrevrange` 实现多优先级任务  高优先级优先出队
3. "优先级+(10000000000000 - 时间戳)"   作为插入的score  实现同score任务先进先出
4. help_list 同步入队任务 并且 出队前先`brpop(0, help_list)` 实现队列为空, 阻塞进程等待. 

### UML图:
当做 **生产者消费者** 设计

 ![](https://ooo.0o0.ooo/2017/06/18/594676a098014.png)