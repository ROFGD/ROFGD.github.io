---
title: JavaSe基础---多线程
date: 2020-05-13 20:42:20
author: ReadPond
tags:
- JavaSe
- 面向对象
categories:
- JavaSe
keywords: 
- 多线程
- Java
top: false
index_img: /img/202301182027348.png
banner_img: /img/202301182027348.png
---

<a name="27fc96f4"></a>

## 1 - Java中的多线程

<a name="7696e88b"></a>

### 1.1 - 进程

> 进程就是正在运行中的应用程序（进程是驻留在内存中）

- 是系统执行资源分配和任务调度的独立单位
- 每个进程都有自己的占用的存储空间和系统资源
    <a name="e213374e"></a>

### 1.2 - 线程

> 线程就是进程中的单个顺序控制流，也可以理解成是一条执行路径

- 单线程：一个进程中包含一个顺序控制流

- 多线程：一个进程中包含多个顺序控制流
    <a name="5ce87190"></a>
    
- >   一、什么是线程，什么是进程
    >
    >   1.进程是一个应用程序，**资源分配的基本单位**
    >
    >   2.线程是一个进程中的执行场景/执行单元；一个进程可以启动多个线程，是**程序执行的基本单位**
    >
    >   二、对于java程序来说，当在DOS窗口输入命令后，会先启动jvm，而jvm就是一个进程，同时再启动一个垃圾回收线程负责回收，至少再目前的java程序中，至少有两个线程并发，一个是垃圾回收线程，一个是执行main方法的主线程
    >
    >   三、**进程****A****和进程****B****的内存独立不共享**
    >
    >   四、**线程****A****和线程****B****，堆内存和方法区内存共享，但是栈内存独立，一个线程一个栈。**假设有十个线程，会有10个栈空间，每个栈都是独立的互不干扰，各自执行各自的，这就是多线程并发。**java****中的多线程机制目的就是为了提高程序的处理效率**
    >
    >   五、**执行一个线程就是执行该线程的****run()****方法中的代码**

### 1.3 - 多线程的实现方式一

-  继承于Thread类<br />1、自定义一个MyThread类，去继承Thread类<br />2、在MyThread类中重写run（）方法<br />3、在测试类中创建MyThread类的对象<br />4、启动线程 

```java
package demo04;

/**
 * @author ly_smith
 * @Description #TODO  自定义线程类
 */
public class MyThread extends Thread{
    public MyThread() {
    }

    public MyThread(String name) {
        super(name);
    }

    //run方法时用来布置多线程任务的方法
    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            System.out.println(this.getName() + "= " + i);
        }
    }
}
----------------------------------------------------------
package demo04;

/**
 * @author ly_smith
 * @Description #TODO  线程测试类
 */
public class Demo04 {
    public static void main(String[] args) {
        //创建自定义线程对象（创建线程）
        MyThread t01 = new MyThread();
        MyThread t02 = new MyThread();
        MyThread t03 = new MyThread("线程03");

        //通过setName方式设置线程名，是补救的方式
        t01.setName("线程01");
        t02.setName("线程02");
        //给线程设置名字
        Thread.currentThread().setName("主线程");


        //启动线程
        t01.start();
        t02.start();
        t03.start();

        for (int i = 0; i < 50; i++) {//Thread.currentThread()  获取当前正在运行的线程对象
            System.out.println(Thread.currentThread().getName() + "= " + i);
        }
    }
}
```

<a name="c2ea4df3"></a>

### 1.4 - 设置和获取线程名

- 设置线程名 
    - `setName(String name)`:设置线程名
    - 通过带参构造方法设置线程名
- 获取线程名 
    - `getName()`:返回字符串形式的线程名
    - `Thread.currentThread()`: 返回正在运行的线程对象
        <a name="e4cd0bce"></a>

### 1.5 - 线程调度

-  线程调度模型 
    - 分时调度模型：所有的线程轮流使用CPU，平均分配每个线程占用CPU的时间。
    - 抢占式调度模型：优先让优先级高的线程使用CPU，如果线程的优先级相同，那么就会随机选择一个线程来执行，优先级高的占用CPU时间可能相对来说会高出那么一点点。

> Java中的JVM使用的就是抢占式调度模型

```java
package demo05;

/**
 * @author ly_smith
 * @Description #TODO  线程调度模型
 */
public class Demo05 {
    public static void main(String[] args) {
        //创建线程
        MyThread t01 = new MyThread("线程01");
        MyThread t02 = new MyThread("线程02");
        MyThread t03 = new MyThread("线程03");

        //分别获取3个线程的优先级，优先级默认为5
//        System.out.println(t01.getPriority());
//        System.out.println(t02.getPriority());
//        System.out.println(t03.getPriority());

        //设置线程优先级
        t01.setPriority(Thread.MIN_PRIORITY);  //小  -  理论上最后执行完毕
        t02.setPriority(Thread.NORM_PRIORITY);
        t03.setPriority(Thread.MAX_PRIORITY);  //大   -  理论上来讲最先执行完毕

        //开启线程
        t01.start();
        t02.start();
        t03.start();
    }
}
```

<a name="36b7cdb6"></a>

### 1.6 - 线程控制

| 方法名                           | 说明                                                    |
| -------------------------------- | ------------------------------------------------------- |
| `void yield()`                   | 使当前线程让步，重新回到争夺队列中                      |
| `static void sleep(long ms)`     | 使当前正在运行的线程停留指定的毫秒数                    |
| `void join()`                    | 等死（等待当前线程销毁后，再执行其它线程）              |
| `void setDaemon(boolean on/off)` | 标记为守护线程，当运行的线程都是守护线程时，JVM停止运行 |

- yield

```java
package demo06;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class MyThread  extends Thread{
    public MyThread() {
    }

    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            if (30 == i){//当i循环到30时，调用yield方法
                Thread.yield();//使当前线程让步
//                                1、让步之后回到争夺队列中马上争夺到执行权，连续执行的效果
//                                2、让步之后回到争夺队列中没有争夺到执行权，让其它线程执行
            }
            System.out.println(this.getName() + ": " + i);
        }
    }
}
-------------------------------------------------
package demo06;

/**
 * @author ly_smith
 * @Description #TODO  yield
 */
public class Demo06 {
    public static void main(String[] args) {
        //创建线程
        MyThread t01 = new MyThread("线程01");
        MyThread t02 = new MyThread("线程02");
        MyThread t03 = new MyThread("线程03");

        //开启线程
        t01.start();
        t02.start();
        t03.start();
    }
}
```

- sleep

```java
package demo07;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class MyThread extends Thread{
    public MyThread() {
    }

    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        for (int i = 1; i < 51; i++) {
            System.out.println(this.getName() + "正在打出第" + i + "招式");
            try {
                //当每个线程遇到sleep方法时都会去停留0.2秒，之后重新回到争夺队列中再次争夺执行权
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
----------------------------------------------------------------
package demo07;

/**
 * @author ly_smith
 * @Description #TODO  sleep
 */
public class Demo07 {
    public static void main(String[] args) {
        //创建线程
        MyThread t01 = new MyThread("黄固");
        MyThread t02 = new MyThread("欧阳锋");
        MyThread t03 = new MyThread("段智兴");
        MyThread t04 = new MyThread("洪七公");

        //开启线程
        t01.start();
        t02.start();
        t03.start();
        t04.start();
    }
}
```

- join

```java
package demo08;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class MyThread extends Thread{
    public MyThread() {
    }

    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        String[] songs = {
            "说天亲，天可不算亲，天有日月和星辰。",
            "日月穿梭催人老，带走世上多少的人。",
            "说天不亲，天也是亲。 天上明月共星辰，",
            "万物生长靠太阳，从古到如今。",
            "说地亲，地也不算亲，地长万物似黄金。",
            "争名夺利有多少载，看罢新坟看旧坟。",
            "说地不亲，地也是亲、 一方水土养一方人，",
            "五谷杂粮把咱养，世上难找回心的人",
            "说爹妈亲，爹妈可不算亲，爹妈不能永生存。",
            "满堂的儿女留也留不住，一捧黄土雨泪纷纷。",
            "说爹妈不亲，爹妈最最亲， 爹妈把我养成人，",
            "为了儿女赞家业，省吃简用不舍半分文。",
            "说儿子亲，不算个亲，人留后代草留根。",
            "八抬大轿把媳妇娶，儿子送给老丈人。"
        };
        for (int i = 0; i < songs.length; i++) {
            System.out.println(this.getName() + "正在唱：" + songs[i]);
        }
    }
}
--------------------------------------------------------
package demo08;

/**
 * @author ly_smith
 * @Description #TODO join
 */
public class Demo08 {
    public static void main(String[] args) throws InterruptedException {
        //创建线程
        MyThread t01 = new MyThread("郭德纲");
        MyThread t02 = new MyThread("郭麒麟");
        MyThread t03 = new MyThread("郭汾阳");

        //开启线程
        t01.start();
        t01.join();//等老郭退休了，两个儿子才有机会唱这个曲儿

        t02.start();
        t03.start();

    }
}
```

- setDaemon

```java
package demo01;

/**
 * @author ly_smith
 * @Description #TODO   自定义线程类
 */
public class MyThread extends Thread{
    public MyThread() {
    }

    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        String[] learn = {
            "人之初，性本善。",
            "性相近，习相远。",
            "苟不教，性乃迁。",
            "教之道，贵以专。",
            "昔孟母，择邻处。",
            "子不学，断机杼。",
            "窦燕山，有义方。",
            "教五子，名俱扬。",
            "养不教，父之过。",
            "教不严，师之惰。",
            "子不学，非所宜。",
            "幼不学，老何为。",
            "玉不琢，不成器。",
            "人不学，不知义。",
            "为人子，方少时。",
            "亲师友，习礼仪。",
            "人之初，性本善。",
            "性相近，习相远。",
            "苟不教，性乃迁。",
            "教之道，贵以专。",
            "昔孟母，择邻处。",
            "子不学，断机杼。",
            "窦燕山，有义方。",
            "教五子，名俱扬。",
            "养不教，父之过。",
            "教不严，师之惰。",
            "子不学，非所宜。",
            "幼不学，老何为。",
            "玉不琢，不成器。",
            "人不学，不知义。",
            "为人子，方少时。",
            "亲师友，习礼仪。--------------------"
        };
        for (int i = 0; i < learn.length; i++) {
            System.out.println(this.getName() + "正在背诵" +learn[i]);
        }
    }
}
-------------------------------------------------
package demo01;

/**
 * @author ly_smith
 * @Description #TODO  setDaemon
 */
public class Demo01 {
    public static void main(String[] args) {
        //创建线程
        MyThread t01 = new MyThread("张三");
        MyThread t02 = new MyThread("李四");
        MyThread t03 = new MyThread("王五");

        //设置守护线程
        t01.setDaemon(true);
        t02.setDaemon(true);
        t03.setDaemon(true);
        //守护没有被标记守护线程的线程

        //启动线程
        t01.start();
        t02.start();
        t03.start();

        for (int i = 1; i < 6; i++) {
            System.out.println("我正在说第" + i +"句话");
        }
        System.out.println("安静，上课了！");
        //当主线程执行完毕后，其它线程不会再争夺新的执行权。
    }
}
```

<a name="9b6af95e"></a>

### 1.7 - 线程的生命周期

- 新建：创建线程对象（通过start方法进入到下一个状态）
- 就绪：有执行资格，但是没有执行权，需要抢CPU执行权
- 运行：有执行资格，并且也有执行权，但是也有可能会被其它线程抢走，如果遇到了阻塞式方法，会在阻塞式方法运行结束之后回到就绪状态，继续争夺CPU执行权
- 死亡：成为垃圾，等待被回收
    <a name="95096397"></a>

### 1.8 - 线程的停止

停止线程意味着线程完成任务之前，需要结束正在执行的操作<br />1、使用标志的方法来退出程序

```java
package demo02;

import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * @author ly_smith
 * @Description #TODO  自定义线程类  使用标志方式停止线程
 */
public class MyThread extends Thread{
    private boolean flag = true;//标志默认为true

    @Override
    public void run() {
        while (flag){
            System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("线程运行结束！~~~~~~~~~");
    }

    //当前方法被调用后，会改变flag的值
    public void stopThread(){
        flag = false;
    }
}
------------------------------------------------------------
package demo02;

/**
 * @author ly_smith
 * @Description #TODO  测试类
 */
public class Demo02 {
    public static void main(String[] args) throws InterruptedException {
        MyThread t01 = new MyThread();//创建线程
        t01.start();//开启线程
        Thread.sleep(2000);
        t01.stopThread();//标记设置为false
    }
}
```

2 - 使用stop方法强制停止线程，导致出现异常，善后工作处理不完整同时会释放锁，可能导致数据不同步，出现数据安全的问题。

```java
package demo03;

import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * @author ly_smith
 * @Description #TODO  自定义线程类
 */
public class MyThread extends Thread{
    @Override
    public void run() {
        try {
            while (true){
                System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));
                Thread.sleep(5000);//每5秒执行一次
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }catch (ThreadDeath e){//捕获stop方法产生的异常
            e.printStackTrace();
        }
    }
}
----------------------------------------------------------
package demo03;

/**
 * @author ly_smith
 * @Description #TODO  stop测试类
 */
public class Demo03 {
    public static void main(String[] args) throws InterruptedException {
        MyThread t01 = new MyThread();
        t01.start();//开启线程
        Thread.sleep(2000);//使主线程睡眠2秒钟
        t01.stop();//调用stop方法强制停止线程
    }
}
```

3 - 使用interruput和isinterrupted方法配合终止线程

```java
package demo04;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class MyThread extends Thread{
    @Override
    public void run() {
        for (int i = 0; i < 10000; i++) {
            if(this.isInterrupted()){//判断正在执行的线程状态是否标记为停止
                System.out.println("已经是停止状态类，退出！");
                break;//调出循环
            }
            System.out.println("i: " + i);
        }
        System.out.println("结束循环后的代码位置");
    }
}
------------------------------------------------
package demo04;

/**
 * @author ly_smith
 * @Description #TODO  线程测试类
 */
public class Demo04 {
    public static void main(String[] args) throws InterruptedException {
        MyThread t01 = new MyThread();//创建线程
        t01.start();
        Thread.sleep(10);
        t01.interrupt();//将线程标记为停止线程
    }
}
```

<a name="425621ac"></a>

### 1.9 - 多线程的实现方式二

-  实现Runnable接口<br />1、自定义MyRunnable类，去实现Runnable接口<br />2、在MyRunnable类中重写run方法<br />3、在测试类中创建Thread类的对象，并将MyRunnable类的对象作为参数传递到Thread类的构造方法中<br />4、启动线程 

```java
package demo05;

/**
 * @author ly_smith
 * @Description #TODO  实现的方式创建多线程
 */
public class MyRunnable implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            System.out.println(Thread.currentThread().getName() + "= " + i);
        }
    }
}
-------------------------------------------
package demo05;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class Demo05 {
    public static void main(String[] args) {
        MyRunnable myRun = new MyRunnable();//构造对象，将任务提取成对象，让多个线程一同使用

        Thread t01 = new Thread(myRun, "线程01");//创建线程
        Thread t02 = new Thread(myRun, "线程02");//创建线程

        //启动线程
        t01.start();
        t02.start();
    }
}
```

- 多线程实现方式 

    - 继承Thread类

    - 实现Runnable接口

            继承是多个线程分别完成自己的任务，虽然执行的是一个类中的方法，但是资源相对独立，相当于创建N个任务由N个线程来执行。<br />         实现是多个线程共同完成一个任务，使用的都是Runnable的资源，他们的资源是共享的，完成的是同一个任务，相当于N个线程执行同1个任务。

        <a name="325654a3"></a>

## 2 - 线程同步

<a name="46f82cc0"></a>

### 2.1 - 卖票案例

需求：德云社北展剧场有一场演出，设置100张特价门票，三个销售点同时卖。

```java
package demo06;

/**
 * @author ly_smith
 * @Description #TODO  线程类
 */
public class SellTickets implements Runnable{
    private int tickets = 100;//设置的100张特价门票

    @Override
    public void run() {
        while (true){
            synchronized (this){//同步语句块
                //卖票动作
                if(tickets > 0){//判断是否有票
                    System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
                    tickets--;//票卖一张少一张
                }
            }
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
----------------------------------
package demo06;

/**
 * @author ly_smith
 * @Description #TODO  卖票测试类
 */
public class SellTicketsTest {
    public static void main(String[] args) {
        SellTickets st = new SellTickets();//提取任务类对象

        //创建线程
        Thread win01 = new Thread(st, "潘家园");
        Thread win02 = new Thread(st, "文化宫");
        Thread win03 = new Thread(st, "回龙观");

        //开启线程
        win01.start();
        win02.start();
        win03.start();
    }
}
```

<a name="241cec3d"></a>

### 2.2 - 数据安全问题

- 是否具备多线程环境
- 是否有共享数据
- 是否有多条语句操作共享数据

> 使用同步语句块`synchronized(任意对象){}`

<a name="804930a6"></a>

### 2.3 - 线程同步的利弊

- 好处：解决线程同步的数据安全问题
- 弊端：当线程很多的时候，每个线程都会去判断同步语句块上的这把锁，很耗费资源，降低运行效率
    <a name="2c16fe32"></a>

### 2.4 - 同步方式

- 同步语句块：`synchronized (this){//同步语句块}`

```java
 synchronized (this){//同步语句块
                //卖票动作
                if(tickets > 0){//判断是否有票
                    System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
                    tickets--;//票卖一张少一张
                }
            }
```

- 普通同步方法：`修饰符 synchronized 返回值类型 方法名（形参列表）{方法体}`

```java
//普通同步方法
    private synchronized void sellTickets() {
        if(tickets > 0){//判断是否有票
            System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
            tickets--;//票卖一张少一张
        }
    }
```

- 静态同步方法：`修饰符 synchronized static 返回值类型 方法名（形参列表）{方法体}`

```java
 //静态同步方法
    private synchronized static void sellTickets() {
        if(tickets > 0){//判断是否有票
            System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
            tickets--;//票卖一张少一张
        }
    }
```

<a name="3fcaaed5"></a>

### 2.5 - Lock

```java
package demo06;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author ly_smith
 * @Description #TODO  线程类
 */
public class SellTickets implements Runnable{
    private static int tickets = 100;//设置的100张特价门票
    private Lock lock = new ReentrantLock();//创建Lock对象，比较灵活

    @Override
    public void run() {
        while (true){
//            synchronized (this){//同步语句块
                 lock.lock();//上锁
                //卖票动作
                if(tickets > 0){//判断是否有票
                    System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
                    tickets--;//票卖一张少一张
                }
//                lock.unlock();//解锁，千万不要忘记解锁，否则其它线程无法进来
//            }

//            sellTickets();//定义普通同步方法

            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
----------------------------------
package demo06;

/**
 * @author ly_smith
 * @Description #TODO  卖票测试类
 */
public class SellTicketsTest {
    public static void main(String[] args) {
        SellTickets st = new SellTickets();//提取任务类对象

        //创建线程
        Thread win01 = new Thread(st, "潘家园");
        Thread win02 = new Thread(st, "文化宫");
        Thread win03 = new Thread(st, "回龙观");

        //开启线程
        win01.start();
        win02.start();
        win03.start();
    }
}
```

<a name="83e1aa18"></a>

### 2.6 - 死锁

- 形成原因：
    - 当两个或多个线程互相锁定时就形成了死锁。
- 避免死锁的原则
    - 顺序上锁，反向解锁

```java
package demo06;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author ly_smith
 * @Description #TODO  线程类
 */
public class SellTickets implements Runnable{
    private static int tickets = 100;//设置的100张特价门票
    private Lock lock = new ReentrantLock();//创建Lock对象，比较灵活
    private Object lock01 = new Object();
    private Object lock02 = new Object();
    private Object lock03 = new Object();
    private Object lock04 = new Object();


    @Override
    public void run() {
        while (true){
            synchronized (lock01){//同步语句块
                if(tickets > 0){//判断是否有票     //t02锁住了lock01
                    synchronized (lock02){
                        System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
                        tickets--;//票卖一张少一张
                    }
                }
            }
            synchronized (lock03){//同步语句块
                //卖票动作
                if(tickets > 0){//判断是否有票
                    synchronized (lock04){      //t01锁住了lock02
                        System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
                        tickets--;//票卖一张少一张
                    }
                }
            }
//            sellTickets();//定义普通同步方法

            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    //普通同步方法
//    private synchronized void sellTickets() {
    //静态同步方法
//    private synchronized static void sellTickets() {
//        if(tickets > 0){//判断是否有票
//            System.out.println(Thread.currentThread().getName() + "正在售出第" + tickets + "号特价门票");
//            tickets--;//票卖一张少一张
//        }
//    }
}
```

<a name="337fd4ba"></a>

## 3 - 生产者与消费者

生产者与消费者模式是并发、多线程编程中经典的设计模式，通过wait和notifyAll方法实现的。<br />简单来说，就是一个类负责生产，一个类负责消费，当生产者生产出商品后，需要等待消费者将商品消费掉。当消费者没有可以消费的商品时，需要等待生产者生产出商品，这其中就存在着线程之间协调配合的过程。

> 针对一个奶箱，送一瓶，才能取一瓶。
> 没送的话，取不到。
> 没取的话，也送不了

```java
package demo01;

/**
 * @author ly_smith
 * @Description #TODO  奶箱类
 */
public class Box {
    private int milk;  //第几瓶牛奶
    private boolean state = false;   //奶箱状态，默认为空

    /**
     * 生产者放牛奶的功能
     * @param milk 第几瓶
     */
    public synchronized void put(int milk){
        //先判断奶箱的状态，如果有牛奶，则需要等待，如果没有牛奶，则需要生产牛奶
        if(state){//如果状态为真，说明有牛奶
            try {
                wait(); //等待，与sleep不同，wait的等待需要有人去唤醒
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //没有牛奶的部分  state=false
        this.milk = milk;
        System.out.println("郭冬临将第：" + this.milk + "瓶牛奶放进了奶箱");
        this.state = true;//修改奶箱状态为真

        notifyAll();//唤醒全部等到的线程
    }

    /**
     * 消费者取牛奶
     */
    public synchronized void get(){
        if(!state){//奶箱状态为假，说明奶箱状态为空
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //有牛奶的部分
        System.out.println("小姐姐将第" + this.milk + "瓶牛奶拿走补了身体");
        this.state = false;//消费者消费完商品后，需要将奶箱状态修改为空

        notifyAll();//唤醒生产者取生产商品
    }
}
---------------------------------------------------
package demo01;

/**
 * @author ly_smith
 * @Description #TODO  生产者线程
 */
public class Producer implements Runnable{
    private Box b;//定义奶箱状态

    public Producer(Box b) {
        this.b = b;
    }

    @Override
    public void run() {
        for (int i = 1; i < 8; i++) {
            b.put(i);//生产者放牛奶
        }
    }
}
-----------------------------------------------------
package demo01;

/**
 * @author ly_smith
 * @Description #TODO  消费者线程
 */
public class Customer implements Runnable{
    private Box b;

    public Customer(Box b) {
        this.b = b;
    }

    @Override
    public void run() {
        while (true){
            b.get();
        }
    }
}
------------------------------------------------------
package demo01;

/**
 * @author ly_smith
 * @Description #TODO  奶箱测试类
 */
public class MilkBoxTest {
    public static void main(String[] args) {
        Box box = new Box();//实例化奶箱类

        Producer producer = new Producer(box);//生产者任务类对象
        Customer customer = new Customer(box);//消费者任务类对象

        Thread tp = new Thread(producer);//生产者线程
        Thread tc = new Thread(customer);//消费者线程

        //启动线程
        tp.start();
        tc.start();
    }
}
```

## 4 - Timer定时器

```java
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Timer;
import java.util.TimerTask;

/**
 * @author ly_smith
 * @Description #TODO  Timer定时器
 */
public class Demo02 {
    public static void main(String[] args) {
        Timer timer = new Timer();//构造定时器对象
        //参数1：布置定时器的任务   参数2：表示启动的时间  参数3：设置循环的时间
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));
            }
        },5000,1000);
    }
}
```

## 5-关于多线程并发环境下，数据的安全问题

-   一、为什么是重点
    -   项目是运行在服务器当中，服务器已经将线程定义，线程对象的创建，线程的启动等，这些代码不需要编写。**需要注重的是数据在多线程并发的环境下是否是安全的**
-   二、什么时候数据在多线程并发的环境下会存在安全问题
    -   1.多线程并发
    -   2.有共享数据
    -   3.共享数据有修改行为
-   三、如何解决线程安全问题
    -   当多线程并发的环境下，有共享数据，并且这个数据还会被修改，此时就存在线程安全问题，解决办法：**线程排队执行****(****不能并发****)**。用排队执行解决线程安全问题，这种机制被称为**线程同步**机制。此时会牺牲一部分效率
-   四、同步编程模型和异步编程模型
    -   **异步编程模型**：线程A和线程B，各自执行各自的，A不需要等待B，B也不需要等待A。其实就是多线程并发(效率较高)
    -   **同步编程模型**：线程A和线程B，在线程A执行的时候必须等待线程B的执行，或者在线程B执行的时候必须等待线程A的执行结束，两个线程之间发生了等待关系，这就是同步编程模型，效率较低，线程排队执行 
-   五、Java中的三大变量
    -   实例变量：在堆内存中
    -   静态变量：在方法区中
    -   局部变量：在栈内存中
    -   以上三大变量中**局部变量永远不会存在线程安全问题**，因为局部变量不共享，实例变量和静态变量，一个在堆内存中一个在方法区中，**堆和方法区都是多线程共享的，所以可能存在线程安全问题**
-   六、如何解决线程安全问题
    -   使用synchronized(共享的对象){
        -   同步线程代码块
    -   }
-   七、在实例方法上可以使用synchronized，并且出现在实例方法上，一定锁的是this，不能是其他对象所以这种方式不灵活。另外还有一个缺点：synchronized出现在实例方法上表示整个方法体需要同步，可能会无故扩大同步范围，导致程序执行效率降低。如果共享的对象是this，并且需要同步的代码块是整个方法体，建议使用这种方式。
    -   synchronized出现在静态方法是类锁，不管创建了几个对象，类锁只有一把
-   八、ArrayList，HashMap,HashSet是非线程安全的；Vector，Hashtable是线程安全的
-   九、synchronized会让程序的执行效率降低，用户体验感不好，系统的用户吞吐量降低，用户体验感差，在不得已的情况下在选择线程同步机制
    -   第一种方案：尽量使用局部变量代替实例变量和静态变量
    -   第二种方案：如果必须是实例变量，那么可以考虑创建多个对象，这样实例变量的内存就不共享
    -   第三种方案：如果不能使用局部变量，对象也不能创建多个，这个时候只能选择synchronized，线程同步机制
