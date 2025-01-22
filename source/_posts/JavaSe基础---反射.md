---
title: JavaSe基础---反射
date: 2020-05-20 20:42:32
author: ReadPond
tags:
- JavaSe
- 反射
categories:
- JavaSe
keywords: 
- 反射
- Java
top: false
index_img: /img/202301182027349.png
banner_img: /img/202301182027349.png
---

<a name="00bbd7ba"></a>

## 1 - 反射

<a name="3315e3c2"></a>

### 1.1 -概述

> Java反射机制就是在程序的**运行**状态中，通过任意一个类的**class**文件，都能获取到这个类的所有属性和方法，这种**动态获取**信息的方式，称之为Java语言的反射机制。

<a name="f79d9c50"></a>

### 1.2 - 获取class对象

```java
package demo06;

/**
 * @author ly_smith
 * @Description #TODO  获取class对象的方式
 */
public class Demo06 {
    public static void main(String[] args) throws ClassNotFoundException {
        //最简单的方式
        Class<Student> c01 = Student.class;
        System.out.println(c01);

        //灵活的方方式
        Class<?> c02 = Class.forName("demo06.Student");
        System.out.println(c02);

        //比较尴尬的方法
        Class<? extends Student> c03 = new Student().getClass();
        System.out.println(c03);
    }
}
```

<a name="e224fbbe"></a>

### 1.4 - 反射获取私有构造方法

```java
package reflex;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class Student {
    public String name;
    public String sex;
    private int age;
    protected float score;
    String tel;

    public Student() {
    }

    public Student(String name, String sex, int age, float score, String tel) {
        this.name = name;
        this.sex = sex;
        this.age = age;
        this.score = score;
        this.tel = tel;
    }

    private Student(String name) {
        this.name = name;
    }

    protected Student(String name, String sex) {
        this.name = name;
        this.sex = sex;
    }

    Student(String name, String sex, int age) {
        this.name = name;
        this.sex = sex;
        this.age = age;
    }
    //自定义成员方法
    public void methodPub(){
        System.out.println("公有成员方法");
    }
    public void methodPub(String info){
        System.out.println("带参公有成员方法");
    }
    private void methodPri(){
        System.out.println("私有成员方法");
    }
    private void methodPri(String info){
        System.out.println("私有带参成员方法");
    }
    protected void methodPro(){
        System.out.println("受保护成员方法");
    }
    protected void methodPro(String info){
        System.out.println("受保护带参成员方法");
    }
    void methodDef(){
        System.out.println("默认成员方法");
    }
    void methodDef(String info){
        System.out.println("默认带参成员方法");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public float getScore() {
        return score;
    }

    public void setScore(float score) {
        this.score = score;
    }

    public String getTel() {
        return tel;
    }

    public void setTel(String tel) {
        this.tel = tel;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", age=" + age +
                ", score=" + score +
                ", tel='" + tel + '\'' +
                '}';
    }
}
----------------------------------------------------------
package reflex;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

/**
 * @author ly_smith
 * @Description #TODO  反射获取私有化构造方法
 */
public class Demo07 {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
//        Student stu = new Student("Andy");//正常的方式不能访问，因为私有的在类外不能访问

        Class<Student> c = Student.class;//获取Student类的class对象

        //获取Student类内部的构造方法，getConstructor只能获取public修改的构造方法
//        Constructor<Student> con = c.getConstructor(String.class);

//        getDeclaredConstructor该方法可以获取修饰符的对象，获取私有化构造方法的对象
        Constructor<Student> con = c.getDeclaredConstructor(String.class);

        //抑制权限检测
        con.setAccessible(true);

        //将构造方法对象进行实例化的过程newInstance
        Student stu = con.newInstance("Andy");

        System.out.println(stu);
    }
}
```

<a name="fe95b7c5"></a>

### 1.5 - 反射获取私有化成员变量

```java
package reflex;

import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;

/**
 * @author ly_smith
 * @Description #TODO  反射获取私有化成员变量
 */
public class Demo08 {
    public static void main(String[] args) throws NoSuchFieldException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class<Student> c = Student.class;//获取Student类的class对象

        //获取私有化的成员变量对象
        Field age = c.getDeclaredField("age");

        //获取公有无参构造方法对象并实例化
        Student stu = c.getConstructor().newInstance();

        //抑制权限检测
        age.setAccessible(true);

        //给成员变量设置值
        //参数1：表示你想修改哪个对象中的成员变量，参数2：设置的实际值
        age.set(stu,18);

        System.out.println(stu);//打印学生对象
    }
}
```

<a name="1b01cc12"></a>

### 1.6 - 反射获取私有化成员方法

```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @author ly_smith
 * @Description #TODO反射获取私有化成员方法并调用
 */
public class Demo01 {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class<Student> c = Student.class;//获取Student类的Class对象

        //获取私有成员方法对象
        Method methodPri = c.getDeclaredMethod("methodPri", String.class);

        //获取公有无参的构造方法对象并实例化
        Student student = c.getConstructor().newInstance();

        //抑制权限检测
        methodPri.setAccessible(true);

        //调用方法
        methodPri.invoke(student,"String");
    }
}
```

<a name="0c8a8fbe"></a>

### 1.7 - 通过反射绕过泛型检测机制

```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.ArrayList;

/**
 * @author ly_smith
 * @Description #TODO   通过反射绕过泛型检测机制
 */
public class Demo02 {
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        ArrayList<Integer> list = new ArrayList<>();//创建集合，用来存储整数类型

        list.add(9527);
//        list.add("9527");
//        list.add(3.14);
//        list.add(3.14f);
//        list.add('A');

        System.out.println("使用反射前：" + list);

        Class<? extends ArrayList> c = list.getClass();//获取集合的class对象

        Method add = c.getMethod("add", Object.class);//获取add方法的对象

        add.invoke(list,"9527");
        add.invoke(list,3.14);
        add.invoke(list,3.14f);
        add.invoke(list,'A');

        System.out.println("使用反射后：" + list);
    }
}
```

<a name="7d6250af"></a>

## 2 - 可变长参数

```java
/**
 * @author ly_smith
 * @Description #TODO  可变长参数
 */
public class Demo03 {
    public static void main(String[] args) {
        System.out.println(mySum(1));
        System.out.println(mySum(1,2,3));
        System.out.println(mySum(1,2,3,4));
        System.out.println(mySum(1,2,3,4,5));
        System.out.println(mySum(1,2,3,4,5,6));
        System.out.println(mySum(1,2,3,4,5,6,7));
        System.out.println(mySum(1,2,3,4,5,6,7,8));
        System.out.println(mySum(1,2,3,4,5,6,7,8,9));
        System.out.println(mySum(1,2,3,4,5,6,7,8,9,10));

//        System.out.println(mySum(mySum(1,2),3));
//        System.out.println(mySum(mySum(mySum(1,2),3),4));
    }

    /*可变长参数可以不传实参，也可以传递多个参数，但是位置必须放在列表的最后面
    * */
    private static int mySum(int b,int ... a) {
        int sum = 0;
        for (int n : a) {
            sum += n;
        }
        return sum;
    }
}
```
