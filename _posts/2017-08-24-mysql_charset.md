---
layout:     post
title:      对mysql字符集设置的记录
tags: mysql 中文乱码
cover: 'https://ws1.sinaimg.cn/large/db141f43gy1fiw0xg4o6bj20nx08x0xc.jpg'
---
# 对mysql字符集设置的记录
## 相关知识:
MySQL的字符集支持(Character Set Support)有两个方面：
> 字符集(Character set)和排序方式(Collation)。

对于字符集的支持细化到四个层次:
> 服务器(server)，数据库(database)，数据表(table)和连接(connection)。

### MySQL默认字符集
MySQL对于字符集的指定可以细化到一个数据库，一张表，一列，应该用什么字符集。
但是，传统的程序在创建数据库和数据表时并没有使用那么复杂的配置，它们用的是默认的配置，那么，默认的配置从何而来呢？
 1. 编译MySQL 时，指定了一个默认的字符集，这个字符集是 latin1；
 2. 安装MySQL 时，可以在配置文件 (my.ini) 中指定一个默认的的字符集，如果没指定，这个值继承自编译时指定的；
 3. 启动mysqld 时，可以在命令行参数中指定一个默认的的字符集，如果没指定，这个值继承自配置文件中的配置,此时 character_set_server 被设定为这个默认的字符集；
 4. 当创建一个新的数据库时，除非明确指定，这个数据库的字符集被缺省设定为character_set_server；
 5. 当选定了一个数据库时，character_set_database 被设定为这个数据库默认的字符集；
 6. 在这个数据库里创建一张表时，表默认的字符集被设定为 character_set_database，也就是这个数据库默认的字符集；
 7. 当在表内设置一栏时，除非明确指定，否则此栏缺省的字符集就是表默认的字符集；

简单的总结一下，如果什么地方都不修改，那么所有的数据库的所有表的所有栏位的都用latin1 存储.

### 查看默认字符集
(默认情况下，mysql的字符集是latin1(ISO_8859_1)
通常，查看系统的字符集和排序方式的设定可以通过下面的两条命令：

```config
mysql> SHOW VARIABLES LIKE 'character%';
mysql> SHOW VARIABLES LIKE 'collation_%';
```
### 修改默认字符集
1. **永久生效的方法:**

    > 注意: 此方法需要删除重新创建已经建立的数据库或者表


    修改my.ini文件
    添加或者修改
    ```config
    [mysql]
    default-character-set=utf8
    ```

    添加或者修改
    ```config
    [mysqld]
    character-set-server=latin1
    ```


2. **如果数据表或者数据库已经建立,不想删除重建**

    查看当前数据表的字符集:
    ```sql
    SHOW CREATE TABLE table_name;
    接着查看CREATE TABLE字段的内容 发现以后有DEFAULT CHARSET信息
    ```
    再修改
    ```sql
    ALTER TABLE table_name CONVERT TO CHARACTER SET utf8;
    ALTER DATABASE database_name DEFAULT CHARACTER SET utf8;
    ```

### 补充点关于字符集的知识
**UTF- 8**：Unicode Transformation Format-8bit，允许含BOM，但通常不含BOM。是用以解决国际上字符的一种多字节编码，它对英文使用8位（即一个字节），中文使用24位（三个字节）来编码。UTF-8包含全世界所有国家需要用到的字符，是国际编码，通用性强。
**GB2312**是GBK的子集，GBK是GB18030的子集
GBK是包括中日韩字符的大字符集合

**gb2312**是简体中文的码
**gbk**支持简体中文及繁体中文
**big5**支持繁体中文
**utf-8**支持几乎所有字符
