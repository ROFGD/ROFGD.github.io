---
title: JavaSe基础---简单网络编程
date: 2020-05-21 20:42:47
author: ReadPond
tags:
- JavaSe
- 网络编程
categories:
- JavaSe
keywords: 
- 网络编程
- Java
top: false
index_img: /img/202301182027345.png
banner_img: /img/202301182027345.png
---

<a name="a4b14711"></a>

## Java中的网络编程

<a name="90e6e30f"></a>

### 1 - 概述

<a name="90cc8474"></a>

#### 1.1 - 网络编程的三要素

1、IP地址<br />2、端口<br />3、协议
<a name="263dd7ad"></a>

#### 1.2 - IP地址

> IP地址就是设备（电脑、手机、ipad、空调、冰箱）在网络中的唯一标识（通过IP地址访问的设备）

-  IP地址的两大分类 
    -  IPV4（主流的网络地址格式） 
    -  IPV6（未来10年够呛能普及） 
-  常用DOS命令 
    - ipconfig -all (查看当前计算机网卡IP地址)
    - ping  xxx.xxx.xxx.xxx (测试网络是否畅通)
    - netstat -ano (查看本机端口占用情况)
-  特殊的IP地址 
    - 127.0.0.1  - 本地的回环地址
        <a name="d97a1097"></a>

#### 1.3 - InetAddress

```java
import java.net.InetAddress;
import java.net.UnknownHostException;

/**
 * @author ly_smith
 * @Description #TODO  InetAddress
 */
public class Demo03 {
    public static void main(String[] args) throws UnknownHostException {
//        InetAddress address = InetAddress.getByName("192.168.1.2");
//        InetAddress address = InetAddress.getByName("127.0.0.1");//通过本地回环地址访问自己
        InetAddress address = InetAddress.getByName("LAPTOP-93CBHP2O");//通过本主机名获取


        System.out.println(address);
        System.out.println(address.getHostName());//获取主机名
        System.out.println(address.getHostAddress());//获取主机地址
    }
}
```

<a name="ca2d935f"></a>

#### 1.4 - 端口

> 端口是一个应用程序（进程/线程）在设备中的唯一标识

端口号：用两个字节的整数来表示（unsigned short :  0 ~ 65535）,其中0 ~ 1024之间的端口进行不要使用，因为很多知名网络服务商和应用程序都会在这个区间选择端口（被占用的几率大），建议使用10000以上端口号，如果被占用，再换一个。
<a name="1fadee68"></a>

#### 1.5 - 协议

> 计算机网络中，连接和通信的规则

-  UDP 
    - User Datagram Protocol  用户数据包协议
    - UDP协议是一种无连接的通信协议，即在数据传输的时候，数据的发送端和接收端不会建立所谓的逻辑连接，简单来说就是当一台计算机向另计算机发送数据时，发送端不会确认接收端是否存在，都会将数据发出。同样接收端也不会再接收到数据后做任何的反馈。
    - UDP协议消耗资源相对小，通信效率高，但是不安全。一般情况下使用在在线的音乐、视频。
-  TCP 
    - Transmission Control Protoclol 传输控制协议
    - TCP协议是一种面向连接/安全的数据通信协议，在数据传输之前，会在发送端和接收端各自创建一个逻辑对象，然后再进行数据的传输。
    - 传输之前会做“三次握手”
    - 传输之后断开连接会做“四次挥手”
        <a name="c6dca399"></a>

## 2 - UDP通信

<a name="c4031eb2"></a>

### 2.1 - 通信原理

> UDP在通信的时候在两端各自创建一个Socekt（套接字）对象，但是这个两个Socket（对象）只是用来发送和接收数据的对象，因此对于UDP协议通信的双方而言，没有所谓的客户端和服务器端的概念。

<a name="53628e69"></a>

### 2.2 - UDP发送

-  发送步骤<br />1、创建发送端的Socket对象 - DatagramSocket<br />2、创建数据，并打包数据<br />3、调用发送方法<br />4、关闭发送 

```java
package demo04;

import java.io.IOException;
import java.net.*;

/**
 * @author ly_smith
 * @Description #TODO  UDP发送
 */
public class UDPSend {
    public static void main(String[] args) throws IOException {
//        1、创建发送端的Socket对象 - DatagramSocket
        DatagramSocket ds = new DatagramSocket();

//        2、创建数据，并打包数据
//        byte buf[], int length,InetAddress address, int port
        byte[] bytes = "Hello UDP Send Messages!~".getBytes();//将字符串转换成byte数组
        int length = bytes.length;//数据长度
        InetAddress address = InetAddress.getByName("127.0.0.1");
        int port = 9527;
        DatagramPacket dp = new DatagramPacket(bytes, length, address, port);

//        3、调用发送方法
        ds.send(dp);

//        4、关闭发送
        ds.close();
    }
}
```

<a name="7764f78d"></a>

### 2.3 - UDP接收

-  接收数据的步骤<br />1、创建接收端Socket对象  - DatagramSoket<br />2、创建一个数据包，用于接收数据<br />3、调用接收方法<br />4、解析数据包<br />5、关闭接收 

```java
package demo04;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

/**
 * @author ly_smith
 * @Description #TODO  UDP接收数据
 */
public class UDPReceive {
    public static void main(String[] args) throws IOException {
//        1、创建接收端Socket对象  - DatagramSoket
        DatagramSocket ds = new DatagramSocket(9527);

//        2、创建一个数据包，用于接收数据
//        byte buf[], int length
        byte[] bytes = new byte[1024];
        int length = bytes.length;
        DatagramPacket dp = new DatagramPacket(bytes, length);

//        3、调用接收方法
        ds.receive(dp);

//        4、解析数据包
        byte[] data = dp.getData();//获取数据包中的数据
        int length1 = dp.getLength();//获取数据包中数据的长度
        System.out.println(new String(data,0,length1));

//        5、关闭接收
        ds.close();
    }
}
```

<a name="13414b28"></a>

### 2.4 - 案例

> 需求：
> UDP发送数据：数据来源于键盘输入，直到输入over的时候就结束发送
> UDP接收数据：因为不知道发送端发送多少次，所以接收端采用死循环

```java
package demo05;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

/**
 * @author ly_smith
 * @Description #TODO  接收端
 */
public class UDPReceive {
    public static void main(String[] args) throws IOException {
        DatagramSocket ds = new DatagramSocket(9527);//创建接收端对象

        while (true){
            byte[] bytes = new byte[1024];//创建容器，用来接收发送过来的数据
            DatagramPacket dp = new DatagramPacket(bytes, bytes.length);//创建接收数据的数据包
            ds.receive(dp);//接收数据
            System.out.println("接收到-" + new String(bytes,0,dp.getLength()));
        }

    }
}
----------------------------------------------------
package demo05;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

/**
 * @author ly_smith
 * @Description #TODO  UDP发送端
 */
public class UDPSend {
    public static void main(String[] args) throws IOException {
        //创建发送端对象
        DatagramSocket ds = new DatagramSocket();
        //自己封装一个获取键盘输入的流
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        String line;
        while ((line = br.readLine()) != null){
            //判断用户是否需要退出，如果输入的是over
            if("over".equals(line))
                break;

            //用户输入的不是over，则需要将数据发送出去
            byte[] bytes = line.getBytes();//将数据转换成byte数组
            InetAddress address = InetAddress.getByName("127.0.0.1");//创建地址
            DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, 9527);
            ds.send(dp);//发送数据
        }

        //关闭发送
        ds.close();
    }
}
```
