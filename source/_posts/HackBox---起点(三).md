---
title: HackBox---起点(三)
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

### HackBox---起点(三)

-   What does the 3-letter acronym SMB stand for?(3 个字母的首字母缩略词 SMB 代表什么？)
    -   Server Message Block
-   What port does SMB use to operate at?(SMB 使用哪个端口进行操作？)
    -   445
-   What is the service name for port 445 that came up in our Nmap scan?(我们的 Nmap 扫描中出现的端口 445 的服务名称是什么？)
    -   microsoft-ds
-   What is the 'flag' or 'switch' we can use with the SMB tool to 'list' the contents of the share?(我们可以使用 SMB 工具来“列出”共享内容的“标志”或“开关”是什么？)
    -   -L
-   How many shares are there on Dancing?(Dancing 有多少个)
    -   5
-   What is the name of the share we are able to access in the end with a blank password?(我们最终能够使用空白密码访问的共享名称是什么？)
    -   WorkShares
-   What is the command we can use within the SMB shell to download the files we find?(我们可以在 SMB shell 中使用什么命令来下载我们找到的文件？)
    -   get
-   获取flag:
-   ![image-20230209234633789](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302092346833.png)

##### 备注：

```
smbclient \\\\10.129.194.55\\WorkShares     #连接用户WorkShares
ls                                          #列出WorkShares目录下信息
cd James.p                                  #进入目录James.p
ls                                          #列出James.p目录下信息
get flag.txt                                #下载文件flag.txt到本地
exit                                        #退出SMB服务
cat flag.txt                                #查看flag.txt
```

##### 参考链接

-   https://www.cnblogs.com/Hekeats-L/p/16535920.html
-   https://docs.oracle.com/cd/E19253-01/820-5488/gfhaq/index.html
