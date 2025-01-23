---
title: HackBox---起点(四)
date: 2022-11-20 20:42:20
author: ReadPond
tags:
- HackBox
categories:
- 靶场
keywords: 
top: true
index_img: /img/202301251815992.png
banner_img: /img/202301251815992.png
---

#### HackBox---起点(四)

-   Which TCP port is open on the machine?(机器上打开了哪个 TCP 端口？)
    -   6379
-   Which service is running on the port that is open on the machine?(哪个服务在机器上打开的端口上运行？)
    -   redis
-   What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database(Redis 是什么类型的数据库？从以下选项中进行选择：(i) 内存数据库，(ii) 传统数据库)
    -   In-memory Database
-   Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.(哪个命令行实用程序用于与 Redis 服务器交互？输入您要在终端中输入的程序名称，不带任何参数。)
    -   redis-cli
-   Which flag is used with the Redis command-line utility to specify the hostname?(Redis 命令行实用程序使用哪个标志来指定主机名？)
    -   -h
-   Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?(连接到Redis服务器后，使用哪个命令获取有关Redis服务器的信息和统计信息？)
    -   info
-   What is the version of the Redis server being used on the target machine?(目标机器上使用的 Redis 服务器的版本是什么？)
    -   5.0.7
-   Which command is used to select the desired database in Redis?(在 Redis 中使用哪个命令来选择所需的数据库？)
    -   select
-   How many keys are present inside the database with index 0?(索引为 0 的数据库中有多少个键？)
    -   4
-   Which command is used to obtain all the keys in a database?(哪个命令用于获取数据库中的所有键？)
    -   keys *

参考链接:

-   https://www.cnblogs.com/Hekeats-L/p/16536247.html
