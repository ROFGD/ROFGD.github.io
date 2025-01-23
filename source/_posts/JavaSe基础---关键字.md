---
title: JavaSe基础---关键字
date: 2020-05-10 15:30:48
author: ReadPond
tags:
- JavaSe
- Java关键字
categories:
- JavaSe
keywords: 
- final
- static
- Java
top: false
index_img: /img/202301191151467.png
banner_img: /img/202301191151467.png
---

### static，final关键字

-   static 关键字修饰变量，可以被类调用--->类名.方法名
-   static 修饰的全局变量称为**类变量**。
-   static 修饰的自己定义的方法称为类方法。
-   static 修饰的代码块称为静态代码块。
-   **static** **修饰的方法不能被重写！！！！**
-   static 不可以修饰的内容：
-   static不能修改类：类惰性加载，static优先分配
-   static不能修饰局部变量
-   static不能修饰set方法 （this当前对象static类方法，不涉及对象）
-   static不能修饰get方法

-   final 关键字表示最终的，不能修改。
-   final 修饰的数据只能看，不能改(普通变量)
-   但是final修饰的类是一个最终的类，不能修改的类，子类继承了父类，基于父类进行了扩展，相当于对父类进行的修改，final修饰的类不能被继承

-   final可以修饰全局变量
    -   全局变量赋值方式：
        -   （1）定义变量时，进行final变量的赋值
        -   （2）final 普通的变量，只能通过对象进行使用，必须创建了对象，才能使用final修饰的变量。只要创建对象，调用构造方法，对象创建成功，通过对象使用变量，使用final类型的变量前，给变量赋值了，就不影响变量的使用。创建对象时，给final变量赋值可以再构造方法中赋值，保证所有的构造方法中，都有给final变量赋值的逻辑
        -   （3）可以再构造代码块中赋值============》代码块执行顺序

-   final修饰局部变量
    -   （1）final可以修饰getset，但是一般不这么用 
    -   （2）final可以修饰我们自己定义的方法，不能被重写
    -   （3）final表示最终的，子类重写父类的方法，对父类中方法覆盖，相当于修改，final修饰的类不能被继承，修饰的方法不能被重写
