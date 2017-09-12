---
layout: post
title: 'Java多线程对Synchronized的理解'
date: 2017-09-12
cover: 'https://ws1.sinaimg.cn/large/db141f43gy1fjh3j8s777j21h30htn3u.jpg'
tags: Java 多线程
---

初次尝试Java多线程编程
因为涉及到在每个线程中保存内容到新文件,文件名的命名是采用递增的方式.所以为了文件名不重复,需要同步文件名这个变量.

`Synchronized`的语法是这样的

```java
synchronized ( xxx ) {
    //code
    //code
}
```

其中的xxx可以填写`this`, `普通成员变量`, `static成员变量`,  `类名.class`

怎么理解Synchronized呢?

### 我的理解是这样的

**首先明确一个概念**
> - 锁 是有 宿主的
> - 每个宿主都对应唯一的一个锁
> - 宿主可以是一个对象,也可以是一个类

**Synchronied语法实际上在做这些事**
1. 检查xxx当前有没有锁
    - 如果没有:对xxx加上一个锁
    -  如果有: 等待锁释放,再对xxx加上一个锁
2. 执行 大括号中的 code 代码
3. 执行完 解除对xxx的锁


**看以下例子, 运行效果写在注释中了**
```java
public class test extends Thread{
    Integer num = new Integer(0);   //普通对象
    static Integer staticNum = new Integer(0);   //static对象
    public void test_this() throws InterruptedException {
        synchronized (this) {
            for (int i = 0; i < 5; i++) {
                System.out.println(this.getName() + " i = " + i);   //各个线程没阻塞
                Thread.sleep(100);
            }
        }
    }
    public void test_normal() throws InterruptedException {
        synchronized (num) {
            for (int i = 0; i < 5; i++) {
                System.out.println(this.getName() + " i = " + i);  //各个线程没阻塞
                Thread.sleep(100);
            }
        }
    }
    public void test_static() throws InterruptedException {
        synchronized (staticNum) {
            for (int i = 0; i < 5; i++) {
                System.out.println(this.getName() + " i = " + i);  //各个线程堵塞
                Thread.sleep(100);
            }
        }
    }
    public void test_class() throws InterruptedException {
        synchronized (test.class) {
            for (int i = 0; i < 5; i++) {
                System.out.println(this.getName() + " i = " + i);  //各个线程阻塞
                Thread.sleep(100);
            }
        }
    }
}
```

### 解释
1. 第一处:`synchronized(this)`没有阻塞访问, 因为每个线程Object的`this`都指自己本身这个Object, 所以事实上是很多歌互相独立的锁;
2. 第二处:`synchronized(num)`没有阻塞,因为`num`每个线程Objdect都是独立的,和`this`一样都是独立的,不同的;
3. 第三处:`synchronized(staticNum)`阻塞,因为static成员变量所有实例公用.
4. 第四处:`synchronized(test.class)`阻塞, 因为test类只有一个


### 总结:
synchronized括号里的对象很重要, 如果是同一个实例, 锁就是同一个.

