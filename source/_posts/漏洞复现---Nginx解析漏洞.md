---
title: 漏洞复现----Nginx解析漏洞
date: 2022-10-14 13:13:56
author: ReadPond
tags: 
- 漏洞
- Nginx
categories:
- 漏洞复现
top: false
index_img: /img/202302201311139.png
banner_img: /img/202302201311139.png
---

### Nginx解析漏洞

#### 一、版本信息

-   Nginx 1.x 最新版
-   PHP 7.x 最新版

#### 二、复现

-   环境：Ubuntu18(靶机)，Win10(攻击机)
-   Ubuntu中进入到Nginx文件夹中，`docker-compose up -d`启动环境
-   在攻击机中访问靶机地址`1.1.1.1/index.php`

![image-20230225190826953](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302251916262.png)

-   选择一个图片，使用文本编辑器在打开图片，最后一行加入一句话木马(图片不宜过大)

![image-20230225190956432](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302251916906.png)

-   选择上传后，访问图片地址，并在图片地址`http://IP/uploadfiles/图片名称.jpg`后面拼上`/.php`或者一个不存在的php文件。

![image-20230225191218679](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202302251916986.png)

-   删除镜像`docker-compose down`

#### 三、分析

-   该漏洞是由于配置不当引起。
-   由于错误配置，Nginx首先会把`.php` 结尾的文件交给`fastcgi`处理，所以才能在图片路径后面拼接一个不存在的php文件。
-   但是`fastcgi`在处理这个不存在的文件时，会受到`php.ini`配置文件中`cgi.fix_pathinfo=1`这个选项的影响(这项配置用于修复路径,如果当前路径不存在则采用上层路径)，到上一级中执行解析(也就是在靶机中`/uploadfiles/d9d99c01b2d14ca7b00c65362f88c7b7.png/aa.php`，`fastcgi`发现`aa.php`文件不存在会对`/uploadfiles/d9d99c01b2d14ca7b00c65362f88c7b7.png`进行解析，`d9d99c01b2d14ca7b00c65362f88c7b7.png`文件是真实存在的)，但是解析时候又受到`php-fpm.conf`中的配置选项`security.limit_extensions`的影响，只有在此选项为空的时候才能指定`.png`等其他文件转为代码解析，如果此选项后面设置参数，那么解析的时候就按设置的参数进行解析，比如：

```
security.limit_extensions = .php .php3 .php4 .php5 
#为了安全，限制能执行的脚本后缀
```

#### 参考链接：

-   https://www.cnblogs.com/0daybug/p/13611542.html

-   https://www.laruence.com/php-internal

    
