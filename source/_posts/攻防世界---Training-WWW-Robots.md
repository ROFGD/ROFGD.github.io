---
title: 攻防世界---Training-WWW-Robots
date: 2023-01-26 13:13:56
author: ReadPond
tags: 
- CTF
categories:
- 靶场
top: false
index_img: /img/202301241049221.png
banner_img: /img/202301241049221.png
---

### Training-WWW-Robots

题目说明：访问链接后，将其翻译过来如图所示，说明这个是有关爬虫robots.txt文件的内容。

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301261159412.png)

所以，我的思路是先尝试访问robots.txt看看能不能找到什么信息，在网址后面拼上robots.txt得到如图内容：

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301261202472.png)

根据文件信息可以看到禁止了所有robots引擎访问f10g.php这个文件，但是允许Yandex搜索引擎访问除过f10g.php文件的所有内容，所以这里看看能不能尝试访问这个被禁止的文件(到了这里能拼在网址后面的只有那个被禁止的文件，所以先尝试拼一下，看能不能行)，访问后得到flag

![](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202301261204350.png)
