---
title: 漏洞原理---PHP特性&缺陷对比函数
date: 2022-10-14 13:13:56
author: ReadPond
tags: 
- PHP危险函数
categories:
- 漏洞复现
top: ture
index_img: /img/202302091327881.png
banner_img: /img/202302091327881.png
---

### PHP特性&缺陷&过滤函数

#### 一、常见过滤函数

-   ==与===

    -   =：赋值

    -   ==：弱类型对比

    -   ===：类型也会对比

    -   ```
        <?php
        $flag = "欢迎吃瓜~~~~";
        $String = "^_^";
        $error = "Waring !!!!";
        if($_GET['name'] != $_GET['password']){
            if(MD5($_GET['name']) == MD5($_GET['password'])){
                echo $flag;
            }else{
                echo $String;
            }
        }else{
            echo $error;
        }
        
        // name = QNKCDZO           0E830400451993494058024219903391
        // Password = 240610708     0E462097431906509019562988736854
        
        <?php
        $flag = "欢迎吃瓜~~~~";
        $String = "^_^";
        $error = "Waring !!!!";
        if($_GET['name'] != $_GET['password']){
            if(MD5($_GET['name']) === MD5($_GET['password'])){
                echo $flag;
            }else{
                echo $String;
            }
        }else{
            echo $error;
        }
        ```

        

    -   ![image-20230308230823166](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202303082308539.png)

    -   ![image-20230308231124204](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202303082311073.png)

-   md5

-   intval

-   strpos

    -   可以使用`%0a`绕过

-   in_array

    -   问题出现在第三个参数上，如果不设置为true，则函数不会检查数据与数组的值类型是否相同

-   preg_match

    -   数组绕过
    -   换行绕过   

-   str_replace

  -   可以使用双写绕过，比如:`x=texttext`
  
  ​    
