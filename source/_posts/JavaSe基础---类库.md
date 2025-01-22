---
title: JavaSe基础---类库
date: 2020-05-01 15:55:59
author: ReadPond
tags:
- JavaSe
- Java常用类库
categories:
- JavaSe
keywords: 
- 类库
- Java
top: false
index_img: /img/202301181547416.png
banner_img: /img/202301181547415.png
---

## 1、类库介绍

### 1.1 - 常用包

java.lang包 -  该包是java中的核心包，该包中的所有类由java虚拟机自动导入

	如：String类、System类、Thread类等

java.util包 - 该包是java中的工具包，该包中提供了大量的工具类和集合类等

	如：Scanner类、Random类、Collections类等

java.io包 - 该包是java中的IO包，该包提供了有关输入输出的类信息等

	如：FileInputStream类、FileOutputStream类等

java.net包 - 该包是java中的网络包，该包中提供了有关网络编程类信息

	如：ServerSocket类、Socket类、DatagramSocket类

### 1.2 - Object类

**基本概念**

> java.lang.Object类是所有类层次结构中的根类


**常用方法**

| 方法名                       | 说明                                       |
| ---------------------------- | ------------------------------------------ |
| `boolean equals(Object obj)` | 用于判断调用对象和参数对象是否相等         |
| `int hashCode()`             | 用于返回调用对象的哈希码值（内存地址编号） |
| `String toString()`          | 用于返回调用对象的字符串表示形式           |


- equals

```java
import java.util.Objects;

/**
 * @author ly_smith
 * @Description #TODO
 */
//如果创建的类没有继承于任何类的情况下，会默认继承于Object类
public class Student{
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
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

    public void setAge(int age) {
        this.age = age;
    }

//重写目的是为了比较对象的内容是否相等
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age &&
                Objects.equals(name, student.name);
    }
}
------------------------------------------------
/**
 * @author ly_smith
 * @Description #TODO  Object类中的equals方法
 */
public class Demo01 {
    public static void main(String[] args) {
        //创建学生对象
        Student stu01 = new Student("Andy", 18);
        Student stu02 = new Student("Andy", 18);

        //当Student类中没有重写equals方法时，默认使用父类Object类中的equals方法，比较两个学生对象的地址
//        System.out.println(stu01.equals(stu02));  //   false

        //因为我们在学生实体类中重写了equals方法，重写的equals方法作用是用来比较两个对象的内容是否相等
        System.out.println(stu01.equals(stu02));  //  true
    }
}
```

- hashCode

```java
/**
 * @author ly_smith
 * @Description #TODO  Object类中的hashCode方法
 */
public class Demo02 {
    public static void main(String[] args) {
        //当前两个字符串对象地址不同
        String str01 = new String("张三");
        String str02 = new String("李四");

        //获取两个对象的哈希码值，哈希值是整数类型
        System.out.println(str01.hashCode());
        System.out.println(str02.hashCode());

        //将str01引用赋值给str03，所以指向同一个地址，所以哈希值相同
        String str03 = str01;
        System.out.println(str03.hashCode());
    }
}
```

- toString

```java
Student类中重写
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
------------------------------------------------
/**
 * @author ly_smith
 * @Description #TODO  Object类中的toString方法
 */
public class Demo03 {
    public static void main(String[] args) {
        //创建学生对象
        Student stu01 = new Student("Andy", 18);

        //调用Student类中的toString方法，因为Student类中没有toString方法，所以调用Object类中的toString方法
        //调用Object类中的toString会打印对象的（包名+类型+@+哈希码无符号十六进制数）
//        System.out.println(stu01.toString());
//        System.out.println(stu01.hashCode());//返回Student对象的哈希码值

        //在学生类中重写toString方法，目的是为了打印对象的内容
        System.out.println(stu01.toString());
        System.out.println(stu01); //toString方法可以自动被调用
    }
}
```

<a name="c9126b0c"></a>

### 1.3 - 包装类

<a name="67cba805"></a>

#### 1.3.1 - 包装类的概念

	在某些场合（集合）中要求所有的内容必须都是对象，但是java中的8中基本数据类型定义的变量并不是对象，为了满足该场合的需求需要对变量做对象化处理，此时需要借助包装类。

<a name="c1fb75cf"></a>

#### 1.3.2 - 常用包装类

| 基本数据类型 | 包装类                |
| ------------ | --------------------- |
| `byte`       | java.lang.Byte类      |
| `short`      | java.lang.Short类     |
| `int`        | java.lang.Integer类   |
| `long`       | java.lang.Long类      |
| `float`      | java.lang.Float类     |
| `double`     | java.lang.Double类    |
| `boolean`    | java.lang.Boolean类   |
| `char`       | java.lang.Charactor类 |

#### 1.3.3 - Integer类

**基本概念**

	java.lang.Integer类被final关键字修饰表示该类不能被继承
	
	该类内部包装了一个int类型的变量作为该类的成员变量，实现int类型的包装

**装箱和拆箱的概念**

	装箱就是指从int类型向Integer类型的转换。
	
	拆箱就是指从Integer类型向int类型的转换。

> 从JDK1.5以后，编译器提供了自动拆箱和装箱的机制


**常用方法**

| 方法名                                | 说明                                                      |
| ------------------------------------- | --------------------------------------------------------- |
| `int intValue()`                      | 用于返回一个调用对象的int类型数据                         |
| `float floatValue()`                  | 用于返回一个调用对象的float类型数据                       |
| `static int parseInt(String str)`     | 用于将参数指定的字符串转换成整数类型数据并返回            |
| `static String toBinaryString(int i)` | 用于将参数指定的int类型数据转换成字符串形式的二进制并返回 |
| `static Integer valueOf(Stirng str)`  | 用于将参数指定的字符串转换为Integer对象                   |


```java
import java.util.ArrayList;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class Demo04 {
    public static void main(String[] args) {
        String str01 = "9527";
        String str02 = "9527acdc";
        int num01 = 123;
        Integer num02 = 456;

        //intValue
        int i = num02.intValue();
        System.out.println(i);
        //floatValue
        float v = num02.floatValue();
        System.out.println(v);
        //parseInt
        int i1 = Integer.parseInt(str01);
//        int i1 = Integer.parseInt(str02);因为该方法不能将字母转换成int类型数据
        System.out.println(i1);
        //toBinaryString
        System.out.println(Integer.toBinaryString(num01));
        //valueOf
        Integer integer = Integer.valueOf(str01);
        System.out.println(integer);
    }
}
```

<a name="56d9a505"></a>

### 1.4 - String类（重点）

**基本概念**

java.lang.String类由final关键字修饰，表示该类不能被继承。

该类用于描述字符串，使用该类创建的对象可以描述Java中的所有字符串字面值，如：“asd”,"321"

```java
注意：
String s1 = null;//没有对象
有何区别？？？？？？
String s1 = ""; //有对象，对象中没有内容
```

**常用构造方法**

| 方法名                                         | 说明                                       |
| ---------------------------------------------- | ------------------------------------------ |
| `String()`                                     | 使用无参的形式来构造对象                   |
| `String(byte[] bytes)`                         | 使用参数指定的字节数组来构造字符串         |
| `String(byte[] bytes, int offset, int length)` | 使用参数指定的字节数组的一部分来构造字符串 |


```java
/**
 * @author ly_smith
 * @Description #TODO  String类的构造方法
 */
public class Demo01 {
    public static void main(String[] args) {
        String str01 = new String();//创建空字符串
        System.out.println(str01);

        byte[] bytes = {97,98,99,100,101,102,103};//初始化byte数组，存储的都是字符编码
        System.out.println(new String(bytes));//将整个byte数组转换成字符串

        //将byte数组中的一部分内容转换成字符串
        System.out.println(new String(bytes,3,3));
    }
}
```

<a name="7d24fec3"></a>

##### 字符串常量池

	Java为了避免产生大量相同的字符串对象，设计了字符串常量池，通过初始化的方式创建的字符串都会存储在常量池中，且字符串不能重复，以便共同使用，提高存储效率。

```java
/**
 * @author ly_smith
 * @Description #TODO  字符串常量池
 */
public class Demo02 {
    public static void main(String[] args) {
        //初始化方式创建字符串
        String str01 = "Hello";//首次存储，先检查字符串常量池中是否存在相同对象的引用存在
                               //没有，则创建对象，将当前对象的引用存储到字符串常量池中，并将引用返回给str01

        String str02 = "Hello";//再次存储，同样先检查字符串常量池中是否有相同对象的引用存在
                               //有，则直接将字符串常量池中的对象引用返回给str02

        System.out.println(str01 == str02);//  true

        String str03 = new String("Hello");//构造的方式创建字符串

        System.out.println(str01 == str03);//false
    }
}
```

##### String类常用方法

| 方法名                                           | 说明                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| `char charAt(int index)`                         | 用于返回调用对象指定参数位置的字符                           |
| `int length()`                                   | 用于返回字符串的长度                                         |
| `int compareTo(String anotherString)`            | 表示按照字典顺序来比较两个字符串的大小                       |
| `int compareToIgnoreCase(String str)`            | 比较字符串大小，忽略大小写                                   |
| `boolean equals(Object anotherObject)`           | 用于判断调用对象字符串和参数对象字符串内容是否相等           |
| `boolean equalsIgnoreCase(Object anotherObject)` | 判断字符串是否相等，忽略大小写                               |
| `boolean contains(CharSequence s)`               | 判断调用对象字符串中是否包含参数字符串                       |
| `String concat(String str)`                      | 返回参数对象与调用对象的拼接                                 |
| `boolean endsWith(String suffix)`                | 判断当前字符串是否以suffix为结尾                             |
| `boolean startsWith(String prefix)`              | 判断当前字符串是否以prefix开头                               |
| `String toLowerCase()`                           | 用于将所有调用对象字符串都转换为小写                         |
| `String toUpperCase()`                           | 用于将所有调用字符串对象转换为大写                           |
| `byte[] getBytes()****`<br />**                  | 用于将字符串内容转换为byte数组并返回                         |
| `int indexOf(String str)`                        | 用于查找指定参数第一次出现的下标，不存在则返回-1             |
| `int lastIndexOf(String str)`                    | 用于查找指定参数最后一次出现的下标。                         |
| `String substring(int beginIndex)`               | 用于获取从参数指定位置开始截取字符串并返回                   |
| `String substring(int beginIndex,int endIndex)`  | 用于获取从beginIndex位置开始到endIndex位置结束之间的字符串。 |


```java
import java.util.Arrays;

/**
 * @author ly_smith
 * @Description #TODO  字符串常用方法
 */
public class Demo03 {
    public static void main(String[] args) {
        String str01 = "Hello JavaSE";
        String str02 = "hello JavaSE";

        //返回字符串中指定位置的字符
        char c = str01.charAt(7);
        System.out.println(c);

        //返回字符串的长度
        System.out.println(str01.length());

        //按照字典顺序比较两个字符串的大小
        System.out.println(str01.compareTo(str02));
        //按照字典顺序比较两个字符串的大小,忽略大小写
        System.out.println(str01.compareToIgnoreCase(str02));

        //该方法时String类内的成员方法，用来比较内容，与Object类中的equals方法不一样。
        System.out.println(str01.equals(str02));
        System.out.println(str01.equalsIgnoreCase(str02));//true

        //判断参数对象是否包含在调用对象中
        System.out.println(str01.contains("Java"));//true
        System.out.println(str01.contains("java"));//false  ，  Java严格区分大小写

        //将调用对象与参数对象拼接的方法,将拼接完成的字符串直接返回
        System.out.println(str01.concat("!~~~"));

        //判断是否以指定的字符串为结尾或者开头
        System.out.println(str01.endsWith("SE"));  //true
        System.out.println(str01.startsWith("hello"));//false

        //将字符串转换为小写和大写
        System.out.println(str02.toLowerCase());
        System.out.println(str02.toUpperCase());

        //将字符串转换成byte数组
        byte[] bytes = str02.getBytes();
        //直接打印数组引用，会打印数组在堆内存中的地址编号
        //想直接查看数组中的内容，需要调用数组工具类Arrays中的toString方法
        System.out.println(Arrays.toString(bytes));

        //查找参数对象在字符串中第一次和最后一次出现的位置
        System.out.println(str02.indexOf("a"));  //7
        System.out.println(str02.lastIndexOf("a"));  //9

        //截取字符串
        System.out.println(str02.substring(6));
        System.out.println(str02.substring(6,10));
    }
}
```

### 1.5 - String类与StringBuilder和StringBuffer区别

	String类型与StringBuilder、StringBuffer的区别主要在于String类构造的对象时不可变的，因此每次对String类型进行改变内容都相当于创建了一个新的对象，而StringBuilder与StringBuffer类的对象能够被多次修改的，并且不产生新的未使用的对象。


| 类型            | 区别                                                         |
| --------------- | ------------------------------------------------------------ |
| `String`        | 值不可变，修改就会创建新的对象，占用内存空间大               |
| `StringBuffer`  | 值可变，不会创建新的对象，占用内存空间小，线程安全，速度慢，多线程 |
| `StringBuilder` | 值可变，不会创建新的对象，占用内存空间小，线程不安全，速度快，单线程 |


```java
/**
 * @author ly_smith
 * @Description #TODO  String类与StringBuilder和StringBuffer的区别
 */
public class Demo04 {
    public static void main(String[] args) {
        //使用String类构造字符串对象
//        String str = new String("Hello");
//        String newStr = str + " Java";
//        System.out.println(str == newStr);


        //使用StringBuilder来构造字符串对象
        StringBuilder str = new StringBuilder("Hello");
        StringBuilder newStr = str.append(" Java");
        System.out.println(str == newStr);//true
        //打印对象
        System.out.println(str);
        System.out.println(newStr);
        //在指定位置插入内容
        newStr.insert(10,"SE");
        System.out.println(str);
        //替换字符串
        str.replace(6,12,"Linux");
        System.out.println(newStr);
        //删除指定位置的字符串
        str.delete(5,11);
        System.out.println(str);
    }
}
```

### 1.6 - 日期相关类

#### 1.6.1 - Date类

	java.util.Date类用于描述日期信息，表示特定的瞬间可以精确到毫秒。

**构造方法**

| 方法名            | 说明                                     |
| ----------------- | ---------------------------------------- |
| `Date()`          | 构造方法用于使用当前系统日期来初始化对象 |
| `Date(long date)` | 构造方法根据参数指定的毫秒数来构造对象   |


**常用方法**

| 方法名                    | 说明                                              |
| ------------------------- | ------------------------------------------------- |
| `long getTime()`          | 用于获取当前对象距离1970年1月1日0:0:0之间的毫秒数 |
| `void setTime(long time)` | 用于根据参数指定的毫秒数来设置时间                |


```java
import java.util.Date;

/**
 * @author ly_smith
 * @Description #TODO  Date类
 */
public class Demo05 {
    public static void main(String[] args) {
        Date date = new Date();//获取当前系统时间来初始化Date对象
        System.out.println(date);//打印时间对象

        long time = date.getTime();//获取从1970.1.1 0:0:0 距离当前调用对象共经历的多少毫秒
        System.out.println(time);//打印共经历的多少毫秒
        //需求：想要获取到去年的当前系统时间的date对象
        //通过整体的时间减掉一年的毫秒数得到去年的当前时间毫秒数
        time = time - 31536000000L;
        Date date1 = new Date(time);//构造去年的当前系统时间的Date对象
        System.out.println(date1);
    }
}
```

#### 1.6.2 - SimpleDateFormat类

**基本概念**

java.text.DimpleDateFormat类用于格式化日期，通俗来说就是调整时间显示的格式。

**常用方法**

| 方法名                                  | 说明                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| `SimpleDateFormat(String pattern)`      | 根据参数指定的格式来构造对象                                 |
| 参数格式                                | “yyyy-MM-dd HH:mm:ss”                                        |
| `public final String format(Date date)` | 用于将参数指定的日期对象按照调用调用对象的格式来转换成字符串形式 |


```java
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * @author ly_smith
 * @Description #TODO  SimpleDateFormat类格式化时间
 */
public class Demo06 {
    public static void main(String[] args) {
        Date date = new Date();//构造当前系统时间的Date对象
        System.out.println(date);

        //定义时间显示的格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String time = sdf.format(date);//将参数对象按照调用对象的格式格式化时间并以字符串形式返回
        System.out.println(time);
    }
}
```

### 1.7 - Math

#### 1.7.1 - 基本概念

	Java中的Math包含了用于执行基本数学运算的属性和方法

#### 1.7.2 - 常用方法

| 方法名                          | 说明                       |
| ------------------------------- | -------------------------- |
| `static int abs(int a)`         | 返回参数的绝对值           |
| `static double ceil(double a)`  | 方法可以对一个数进行上舍入 |
| `static double floor(double a)` | 方法可以对一个数进行下舍入 |
| `static int round(double a)`    | 返回一个四舍五入的值       |
| `static int min(int a , int b)` | 返回两个参数的最小值       |
| `static int max(int a, int b)`  | 返回两个参数的最大值       |


```java
/**
 * @author ly_smith
 * @Description #TODO  Math类中的常用方法
 */
public class Demo01 {
    public static void main(String[] args) {
        int n01 = -18;
        int n02 = 21;
        double d01 = 3.4;
        double d02 = 3.5;

        //abs获取参数的绝对值
        System.out.println(Math.abs(n01));
        System.out.println(Math.abs(n02));

        //四舍五入
        System.out.println(Math.round(d01));
        System.out.println(Math.round(d02));

        //上舍入
        System.out.println(Math.ceil(d01));
        System.out.println(Math.ceil(d02));

        //下舍入
        System.out.println(Math.floor(d01));
        System.out.println(Math.floor(d02));

        //比较两个参数的最大值和最小值
        System.out.println(Math.min(n01,n02));
        System.out.println(Math.max(n01,n02));
    }
}
```

### 1.8 - Scanner

#### 1.8.1 - 基本概念

	一个简单的文本扫描器，用final修饰的，表示该类不可被继承

#### 1.8.2 - 基本使用

```java
import java.util.Scanner;

/**
 * @author ly_smith
 * @Description #TODO  Scanner文本扫描器
 */
public class Demo02 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);//构造扫描器对象

//        System.out.println("请输入整数类型的数据：");
//        System.out.println(sc.nextInt() + "~~~");

//        System.out.println("请输入float类型的数据：");
//        System.out.println(sc.nextFloat());

//        System.out.println("请输入boolean类型数据：");
//        System.out.println(sc.nextBoolean());

//        System.out.println("请输入一串字符串：");
//        //当读取到有效字符后才可以结束输入，空格和回车会自动忽略
//        System.out.println(sc.next());

        //以Enter键为结束符，可以识别空格
        System.out.println("请输入一串字符串：");
        System.out.println(sc.nextLine());
    }
}
```

### 1.9 - Random

#### 1.9.1 - 基本概念

	该类是Java中用来生成随机数的类。

#### 1.9.2 - 基本引用

```java
import java.util.Random;

/**
 * @author ly_smith
 * @Description #TODO  Random随机类
 */
public class Demo03 {
    public static void main(String[] args) {
        Random ran = new Random();//创建随机类对象

        //生成一个随机数,默认使用int类型的范围
        System.out.println(ran.nextInt());

        //设置一个固定的范围,设置100则会在0 ~ 99之间选择一个数
        System.out.println(ran.nextInt(100));

        //返回浮点数类型的数据
        System.out.println(ran.nextDouble());
        System.out.println(ran.nextFloat());
    }
}
```
