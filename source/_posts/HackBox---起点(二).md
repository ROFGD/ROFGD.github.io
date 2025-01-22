---
title: HackBox---起点(二)
date: 2022-11-20 20:42:20
author: ReadPond
tags:
- HackBox
categories:
- 靶场
keywords: 
top: true
index_img: /img/202301181833979.png
banner_img: /img/202301181833979.png
---

### HackBox---起点(二)

-   What does the 3-letter acronym FTP stand for?(3 个字母的首字母缩略词 FTP 代表什么？)
    -   File Transfer Protocol(文件传输协议)
-   Which port does the FTP service listen on usually?(FTP服务通常监听哪个端口？)
    -   21
-   What acronym is used for the secure version of FTP?(FTP 的安全版本使用什么首字母缩写词？)
    -   SFTP
-   What is the command we can use to send an ICMP echo request to test our connection to the target?(我们可以使用什么命令发送 ICMP 回显请求来测试我们与目标的连接？)
    -   ping
-   From your scans, what version is FTP running on the target?(根据您的扫描结果，目标上运行的 FTP 版本是什么？)
    -   vsftpd 3.0.3
-   From your scans, what OS type is running on the target?(根据您的扫描结果，目标上运行的操作系统类型是什么？)
    -   Unix
-   What is the command we need to run in order to display the 'ftp' client help menu?(为了显示“ftp”客户端帮助菜单，我们需要运行什么命令？)
    -   ftp -h
-   What is username that is used over FTP when you want to log in without having an account?(当您想在没有帐户的情况下登录时，通过 FTP 使用的用户名是什么？)
    -   anonymous
-   What is the response code we get for the FTP message 'Login successful'?(我们得到的 FTP 消息“登录成功”的响应代码是什么？)
    -   230
-   There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.(我们可以使用几个命令来列出 FTP 服务器上可用的文件和目录。一个是目录。另一种是在 Linux 系统上列出文件的常用方法。)
    -   ls
-   What is the command used to download the file we found on the FTP server?(用于下载我们在 FTP 服务器上找到的文件的命令是什么？)
    -   get

-   获取flag

-   ![image-20230209222253771](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302092222339.png)

    ![image-20230209222507154](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302092225390.png)
