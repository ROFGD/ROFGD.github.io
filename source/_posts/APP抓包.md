---
title: APP抓包
date: 2023-03-08 15:53:13
author: ReadPond
tags: 
- APP
- 抓包
categories:
- 移动端
top: ture
index_img: /img/202301241049221.png
banner_img: /img/202301241049221.png
---

### APP抓包

#### 一、Charles抓包步骤

-   环境：Charles 4.6.3   逍遥模拟器
-   步骤：
    -   Charles中下载证书:`Help–>SSL Proxying—>Save Charles Root Certificate…`
    -   将证书保存到本地，然后将证书放入到与模拟器共享的文件夹下(路径无所谓，只要是与模拟器共享的文件夹就行)
    -   ![image-20230308160627308](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202303081606177.png)
    -   在模拟器的找到设置->安全->从SDK安装->找到刚刚存放证书的文件(我是放到音乐文件夹里，所以得在模拟器中找Music这个文件夹)
    -   ![image-20230308161109823](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202303081611075.png)
    -   ![image-20230308161126263](C:\Users\ReadPond\AppData\Roaming\Typora\typora-user-images\image-20230308161126263.png)
    -   ![image-20230308161141735](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202303081611688.png)
    -   点击该文件进行安装即可

#### 二、Burp抓包

-   ![image-20230308163034121](https://cdn.jsdelivr.net/gh/ROFGD/Drawingbed/202303081630868.png)
-   保存后，需要将证书后缀更改为`Burp.cer`，模拟器中无法识别`Burp.der`后缀，更改完后，安装与上面安装相同

#### 三、抓包当中遇到的问题

-   在模拟器中测试时，设置代理后，通过代理能正常访问浏览器，并且Burp中有数据包测试抓包正常情况下，打开某个APP后发现抓包工具中没有任何数据包返回，同时APP中有`网络连接失败`等相关网络提示。可能存在以下情况： 
    -   此APP内部有反代理机制
        -   自身抓包应用
        -   使用Proxifier PE转发
            -   大致原理：如果设置了系统代理app->代理服务器->burp->服务端，APP检查模拟器或者手机的代理设置，发现设置代理则会出现网络连接异常。但是如果使用Proxifier PE软件进行转发(app->Proxifier PE->本地burp->服务)，则不需要设置系统代理，相当于(模拟器的网络出口时通过本机进行的)将APP的流量流向Proxifier PE，通过Proxifier PE将流量传递出去。
    -   证书问题，如果代理开的是http代理，但是APP数据走的是SLL/https
        -   情况1，客户端不存在证书校验，服务器也不存在证书校验。
        -   情况2，客户端存在校验服务端证书，服务器也不存在证书校验，单项校验。
        -   情况3、客户端存在证书校验，服务器也存在证书校验，双向校验。
            -   抓包使用Burp等抓包工具时，证书使用的是抓包工具自身的证书，并不是APP本身的证书，所以双向校验时前后端证书不一致就会出现开了http代理app不能正常运行
