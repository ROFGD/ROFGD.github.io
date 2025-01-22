---
title: JavaSe基础---GUI设计
date: 2020-05-16 10:26:44
author: ReadPond
tags:
- GUI
categories:
- JavaSe
keywords:
- 界面设计
top: true
index_img: /img/202301191151469.png
banner_img: /img/202301191151469.png
---

### GUI设计

-   1.Swing工具包下有MVC结构，所谓MVC结构就是：模型(数据)+视图(界面)+控制(监听事件)

-   2.JFrame窗体

    -   构造JFrame对象

    -   设定窗体宽高setSize()

    -   设定窗体可见setVisible()

    -   设定关闭方式(根据情况设定)

    -   setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE)

-   DO_NOTHING_ON_CLOSE”、“DISPOSE_ON_CLOSE”、“HIDE_ON_CLOSE”、“EXIT_ON_CLOSE”。
    -   第一种表示什么也不做就将窗体关闭；
    -   第二种表示任何注册监听程序对象后会自动隐藏并释放窗体；
    -   第三种表示隐藏窗口的默认窗口关闭；
    -   第四种表示退出应用程序默认窗口关闭。

-   3.Swing包
    -   文本框----JTextField
    -   密码框----JPasswordField
    -   标签----JLable
    -   复选框-----jCheckBox
    -   单选框-----JRadioButton(同一组单选按钮必须先创建ButtonGroup，然后把单选框组件放入到ButtonGroup中)

### 登录界面Demo

#### 一、思路

-    (1)用到的窗体
    -    这次项目中使用的窗体为JFrame，实现图形界面，首先必须有一个顶级窗体
-    (2) 使用到的标签
    -   Jlabel 标签元素类，显示文字和图片
    -   JTextField 文本输入框元素组件类，接收用户输入信息并将其显示
    -   JPasswordField 密码输入框组件类，接收用户输入的信息，然后把每一个字符都用一个加密符号显示
    -   JButton 按钮元素组件类，显示文字或图片，也可以一起显示，可以点击
-    (3) 使用到的布局
    -   java.awt.FlowLayout   流式布局类
    -   java.awt.Dimension   封装组件宽度和高度的类
-   布局类是针对容器组件的，它会让添加到容器上的组件按照布局类的方式去排列对齐。
-   如果我们没有设置窗体的布局，那么我们添加的组件就会出现覆盖的问题，最后只会显示最后添加的那个组件流式布局的效果类似于word文档，对组件按行进行排列，当前行满了再放到下一行。但是不能像word一样回车换行。

#### 二、项目逻辑

-   创建JFrame窗体
    -   1.设置窗体文字
    -   2.设置窗体大小
    -   3.设置窗体大小不可调(**setResizable(false)**)
    -   4.设置窗体相对于另一个窗体居中位置(**setLocationRelativeTo(null)**)
    -   5.设置窗体关闭
    -   6.设置窗体居中显示
-   使用流式布局
    -   1.创建流式布局对象并实例化
    -   2.设置对齐方式为居中，组件间隔为10(视情况而定)
-   设置除顶级窗体大小外，其他**组件**大小
    -   Dimension(hight,weight)
-   创建JLabel标签并添加到窗体
    -   1.创建用户名和密码标签
    -   2.添加到窗体
-   创建JTextField和JPasswordField组件并添加到窗体
    -   1.创建输入文本框和密码框对象
    -   2.调用setPreferredSize()方法设置文本框和密码框大小
    -   3.添加到窗体
-   创建JButton组件并添加到窗体
    -   1.创建登录和注册按钮标签
    -   2.调用setText()方法在按钮上显示登录和注册
    -   3.添加到窗体
-   使窗体可显示

```java
import javax.swing.*;
import java.awt.*;

public class Login extends Frame {
    public void Jframe() {
        //设定窗体容器
        JFrame frame = new JFrame();
        frame.setTitle("社团成员登录界面"); //容器文字
        frame.setSize(350,120);	//容器大小
        frame.setResizable(false);	// 设置禁止调整窗体大小
        frame.setLocationRelativeTo(null);// 设置窗体相对于另一个组件的居中位置，		
        frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);   	               //容器关闭
        frame.setLocationRelativeTo(null);	//居中显示

        //使用流式布局，对齐方式居中，组件间隔为10
        FlowLayout fl = new FlowLayout(FlowLayout.CENTER,10,10);
        frame.setLayout(fl);

        //Dimension 类封装单个对象中组件的宽度和高度（精确到整数）
        Dimension dim1 = new Dimension(100,20);

        //创建标签对象，该对象显示为用户名，并将其添加到窗体上
        JLabel lbUser = new JLabel("用户名：");
        frame.add(lbUser);
        //创建用户名文本框
        TextField text_name = new TextField();
        text_name.setPreferredSize(dim1);	//设置除顶级容器组件其他组件的大小
        frame.add(text_name);	//添加到容器

        //创建标签对象，该对象显示为密码，并将其添加到窗体上
        JLabel lbPass = new JLabel("密码：");
        frame.add(lbPass);
        //创建密码文本框
        JPasswordField text_pass = new JPasswordField();
        text_pass.setPreferredSize(dim1);
        frame.add(text_pass); //！！！注意：这里必须是创建一个标签对应创建一个相应文本框
        
        //设置登录注册按钮
        JButton btn_Up = new JButton();
        btn_Up.setText("登录");	//调用setText()方法在按钮上显示登录
        frame.add(btn_Up);

        JButton btn_Sign = new JButton();
        btn_Sign.setText("注册");	//调用setText()方法在按钮上显示注册
        frame.add(btn_Sign);

        //使窗体可见
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        new Login().Jframe();
    }
}

```

#### 参考链接：

1.  https://blog.51cto.com/javanew/1956117
2.  https://blog.51cto.com/javanew/1955440
