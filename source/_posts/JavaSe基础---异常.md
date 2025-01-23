---
title: JavaSe基础---异常
date: 2020-05-02 09:21:23
author: ReadPond
tags:
- JavaSe
- 面向对象
categories: 
- JavaSe
index_img: /img/202301181043439.png
banner_img: /img/202301181103911.png
top: true
---

## 1 - 异常

### 1.1 - 基本概念

	异常，指不正常，在Java中异常处理机制能让程序在异常情况发生时按照事先设定好的逻辑方式去有针对性的处理异常的方式。

### 1.2 - 异常的主要分类

java.lang.Throwable类java中所有错误和异常的超类，已知直接子类有Error类和Exception类。

| 名称               | 说明                           |
| ------------------ | ------------------------------ |
| `RuntimeException` | 运行时异常，也叫作非检测性异常 |
| `IOException`      | IO异常类，也叫作检测性异常     |


**RuntimeException主要子类**

| 异常类型                         | 说明             |
| -------------------------------- | ---------------- |
| `ArithmeticException`            | 算数异常         |
| `ArrayIndexOutofBoundsException` | 数组下标越界异常 |
| `NullPoniterException`           | 空指针异常       |
| `ClassCastException`             | 类型转换异常     |
| `NumberFormatException`          | 数字格式异常     |


**注意：**

	当程序执行过程中产生异常并没有手动处理时，则采用默认的处理方式，打印异常名称、异常原因、异常的位置等信息，并终止程序，导致后续代码无法运行。

**运行时异常的处理**

	绝大多数的运行时异常都可以通过if判断的形式将其避免发生。

```java
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  RuntimeException 的主要子类
 */
public class Demo02 {
    public static void main(String[] args) {
        //针对于运行时异常有两种处理方式，1、通过if逻辑判断避免其发生  2、通过异常的捕获格式捕获异常
        int a = 10;
        int b = 0;
//        if (b != 0){//当除数不为0，才可以运行
//            System.out.println(a/b);
//        }
        //通过异常捕获的方式处理运行时异常,虽然捕获也可以处理运行时异常类，
        // 但是针对于运行时异常，通常需要使用if判断的方式将其避免发生
//        try {
//            System.out.println(a/b);
//        }catch (ArithmeticException e){
//            e.printStackTrace();
//        }

        int[] arr = new int[5];
        int pos = 5;
        if (pos < 5 && pos >= 0){
            System.out.println(arr[pos]);//java.lang.ArrayIndexOutOfBoundsException
        }

        String s1 = null;
        if (s1 != null){
            System.out.println(s1.length());//java.lang.NullPointerException
        }

        Exception e = new Exception();
        if (e instanceof IOException){
            IOException e1 = (IOException) e;//java.lang.ClassCastException
        }

        String s2 = "123abc";
        if (s2.matches("\\d+")){
            System.out.println(Integer.parseInt(s2));//java.lang.NumberFormatException
        }
    }
}
```

<a name="a148184a"></a>

### 1.3 - 异常的捕获格式

```java
try{
  编写可能会产生异常的语句；
}catch(异常类型 变量名){
  编写针对该异常的处理语句；
}
```

<a name="e062b3a8"></a>

### 1.4 - 异常的捕获以及执行流程

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

/**
 * @author ly_smith
 * @Description #TODO  异常的捕获以及执行流程
 */
public class Demo03 {
    public static void main(String[] args) {
        try {
            System.out.println("a");
            FileInputStream fis = new FileInputStream("./a.txt");
            System.out.println("b");
        }catch (FileNotFoundException e){
            e.printStackTrace();
            System.out.println("c");
        }finally {
            //finally的语句块，表示无论异常是否发生都会执行的语句块
            System.out.println("d");
        }

        System.out.println("e");

        //当异常出现时的执行流程：a  c  d  e
        //当异常未出现的执行流程：a  b  d  e
    }
}
```

当上述代码没有发生异常时的执行流程是：a  b  d  e

当上述代码发生异常时的执行流程是:	a  c  d  e

finally中通常编写无异常是否发生都应该执行的代码，因此通常来做善后处理，比如:关闭文件，断开数据库连接等操作

**注意事项**

当需要catch多种不同类型的异常时，切记晓得类型需要放在大的类型的上面

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  当多catch中需要捕获多种异常类型时，小类型放上面
 */
public class Demo05 {
    public static void main(String[] args) {
        try {
            FileInputStream fis = new FileInputStream("./aa.txt");
            fis.close();
        }/* catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e){
            e.printStackTrace();
        }*/ catch (Exception e){
            e.printStackTrace();
        }
        //小的类型需要放在上面，大的类型需要放在下面
    }
}
```

### 1.5 - 手动抛出异常

	在某些特殊的场合中，对于出现的异常无法直接处理或者不便于处理时，就可以选择将异常转移给方法的调用者，这种形式就叫做异常的抛出。

**语法格式：**

```java
public void show() throws IOException{方法体}
```

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

/**
 * @author ly_smith
 * @Description #TODO  异常的抛出
 * */
public class Demo06 {
    private static void show() throws FileNotFoundException {
        FileInputStream fis = new FileInputStream("./a.txt");
    }

    public static void main(String[] args) {
        try {
            show();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        //一旦异常抛到了主方法中时，建议将其进行捕获，原因如果将异常再次抛出则会抛给JVM，
        // 会给JVM造成负担，导致程序运行效率的降低
    }
}
```

	在子类中，如果父类中被重写的方法抛出了异常，那么子类中重写的方法可以抛出更小的异常、一样的异常，不可以抛出更大的异常。

```java
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  子类中重写父类方法时可以排除的异常类型 父类
 */
public class Demo07 {
    public void show() throws IOException{
        System.out.println("我是Demo07中的show方法");
    }
}
--------------------------------------------------------
import java.io.FileNotFoundException;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  Demo07的子类
 */
public class Demo07Sub extends Demo07{
    //子类可以抛出与父类相同的异常类型
    /*@Override
    public void show() throws IOException {

    }*/

    //子类可以抛出比父类更小的异常类型
    /*@Override
    public void show() throws FileNotFoundException{

    }*/
    //子类中不可以排除比父类更大的异常
   /* @Override
    public void show() throws Exception{

    }*/
}
```

### 1.6 - 自定义异常

	自定义异常类去继承于Exception类或其子类，提供两个版本的构造器。

```java
/**
 * @author ly_smith
 * @Description #TODO  自定义异常类
 */
public class AgeException extends Exception{
    public AgeException() {
    }

    //传递异常的原因
    public AgeException(String message) {
        super(message);
    }
}
------------------------------------------
/**
 * @author ly_smith
 * @Description #TODO
 */
public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        setName(name);
        try {
            setAge(age);
        } catch (AgeException e) {
            e.printStackTrace();
        }
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) throws AgeException {
        //设置年龄的范围，如果不满足要求，则直接创建异常并抛出
        if(age < 0 || age > 150){//年龄不合理，则创建异常对象，并抛出
            throw new AgeException("年龄不合理，请重新输入！");
        }else{
            //年龄合理的部分，则直接赋值即可
            this.age = age;
        }

    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
------------------------------------------------------------
/**
 * @author ly_smith
 * @Description #TODO  自定义类的测试类
 */
public class Demo08 {
    public static void main(String[] args) {
        Student stu = new Student("Andy", -18);//构造一个学生对象
        System.out.println(stu);
    }
}
```

<a name="a910e165"></a>

### 1.7 - final、finally、finalize的区别

| 方法名     | 说明                                               |
| ---------- | -------------------------------------------------- |
| `final`    | 修饰的变量不可被修改，方法不可被重写，类不可被继承 |
| `finally`  | 异常处理机制中的一个关键字                         |
| `finalize` | 垃圾收集器执行之前被调用的方法                     |


```java
/**
 * @author ly_smith
 * @Description #TODO  final、finally、finalize区别
 */
public /*final*/ class Demo01 {//final修饰的类，表示不能被继承
    public final static String NAME = "Andy";

    public final void show(){//用final修饰的方法不可被重写
        System.out.println("我是被final修饰的方法");
    }

    @Override
    protected void finalize() throws Throwable {
        System.out.println("我被调用了");
    }

    public static void main(String[] args) {
//        NAME = "Bob";//被final修饰的变量表示常量，不可被修改
        Demo01 demo01 = new Demo01();
        demo01 = null;
        System.gc();//启动垃圾收集器
    }
}
-------------------------------
/**
 * @author ly_smith
 * @Description #TODO  Demo01的子类
 */
public class Demo01Sub extends Demo01{
    /*@Override
    public void show() {
        super.show();
    }*/
}
```

