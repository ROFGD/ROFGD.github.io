---
title: JavaSe基础---集合
date: 2020-05-10 20:42:10
author: ReadPond
tags:
- JavaSe
- 集合
categories:
- JavaSe
keywords: 
- 集合
- Java
top: false
index_img: /img/202301182027346.png
banner_img: /img/202301182027346.png
---

<a name="70d6a007"></a>

## 1 - 集合

<a name="e2427894"></a>

### 1.1 - 概述

- 在内存中申请一块空间用来存储数据，在Java中集合实际上就是替换掉定长数组的一种引用数据类型
    <a name="yfl7X"></a>

### 1.2 - 集合与数组的区别

- **长度区别**
    - 数组长度固定，定义长了会造成内存空间的浪费，定义短了内存空间不够用
    - 集合大小可变，用多少拿多少空间
- **内容区别**
    - 数组可以存储基本数据类型和引用数据类型
    - 集合只能存储引用数据类型
- **元素区别**
    - 数组只能存储同一种类型的数据
    - 集合中可以存储不同类型的数据（一般情况下也只存同一种类型的数据）
- **集合结构**
    - 单列集合  **Collection** 
        - **List**可以重复：_ArrayList/LinkedList_
        - **Se**t不可重复:_HashSet/TreeSet_
- 双列集合 **Map**：_HashMap/TreeMap_

> 接口：粗体    实现类：斜体

<a name="0fabf3cb"></a>

### 1.3 - ArrayList

`ArrayList<E>`原型

- ArrayList是一个List接口的实现类
- 底层就是一个可以调整大小的数组实现的
- `<E>`：是一种特殊的数据类型 （引用数据类型） - 泛型 
    - `ArrayList<String> 或者 ArrayList<Student> 或者 ArrayList<Integer>`
        <a name="43c8c596"></a>

### 1.4 - ArrayList构造和添加方法

| 方法名                             | 说明                                     |
| ---------------------------------- | ---------------------------------------- |
| `public ArrayList<E>()`            | 创建一个空的集合                         |
| `public boolean add(E e)`          | 将指定的元素追加到集合的末尾             |
| `public void add(int index , E e)` | 在集合的指定位置插入元素                 |
| `public void addAll(E object)`     | 用于将指定集合中所有元素添加到当前集合中 |


```java
import java.util.ArrayList;

/**
 * @author ly_smith
 * @Description #TODO  ArrayList集合的创建和添加
 */
public class Demo03 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();//创建空集合

        //添加元素到list集合中，成功则返回true
        System.out.println(list.add("刘德华"));
        System.out.println(list.add("张学友"));
        System.out.println(list.add("郭富城"));
        System.out.println(list.add("黎明"));

        //指定位置插入元素
        list.add(2,"周润发");
//        list.add(12,"谭咏麟");//使用索引插入元素时，索引的最大值不能大于集合中元素个数

        //向指定集合中添加另一个集合中的全部元素
        ArrayList<String> newList = new ArrayList<>();

        newList.add("宋小宝");
        newList.add("小沈阳");
        newList.add("刘能儿");
        newList.add("赵四儿");

        //将参数集合中全部元素添加到调用集合中
//        list.addAll(newList);//追加的方式
        list.addAll(3,newList);//指定位置插入元素

        //查看集合
        System.out.println(list);
    }
}
```

<a name="0dd77d36"></a>

### 1.5 - ArrayList集合的常用方法

| 方法名                            | 说明                                       |
| --------------------------------- | ------------------------------------------ |
| `public boolean remove(Object o)` | 删除指定的元素，成功则返回true             |
| `public E remove(int index)`      | 删除指定索引位置的元素，返回被删除的元素   |
| `public E set(int index , E e)`   | 修改指定索引对应的元素，返回被修改前的元素 |
| `public E get(int index)`         | 获取指定索引位置的元素                     |
| `public int size()`               | 返回集合的元素个数                         |


```java
import java.util.ArrayList;
import java.util.Iterator;

/**
 * @author ly_smith
 * @Description #TODO  集合中常用方法
 */
public class Demo04 {
    public static void main(String[] args) {
        //创建集合
        ArrayList<String> list = new ArrayList<>();

        //追加方式添加元素
        list.add("东邪");
        list.add("西毒");
        list.add("南帝");
        list.add("北丐");
        list.add("中神通");

        System.out.println("操作前：" + list);

        //通过元素的名称进行删除
        System.out.println(list.remove("西毒"));
        //通过索引位置删除元素
        System.out.println(list.remove(3));

        //修改指定元素，并将被删除的元素进行返回
        System.out.println(list.set(1,"西毒"));

        //获取指定索引位置的元素
        System.out.println(list.get(2));

        System.out.println("操作后：" + list);

        //集合的遍历，普通for循环，foreach，迭代器，Stream
        //方法一
        for (int i = 0; i < list.size(); i++) {
            System.out.print(list.get(i) + "\t");
        }
        System.out.println();

        //方法二
        for (String str : list) {
            System.out.print(str + "\t");
        }
        System.out.println();

        //方法三
        Iterator<String> it = list.iterator();//创建迭代器对象
        while (it.hasNext()){//判断下一个位置是否有元素，有元素则返回true
            System.out.print(it.next() + "\t");//取出下一个位置的元素，并将游标移动到当前位置
        }
        System.out.println();

        //方法四
        list.stream().forEach(System.out::println);
    }
}
```

<a name="fbf2da13"></a>

### 1.6 - ArrayList存储学生对象并格式化输出

```java
import java.util.ArrayList;

/**
 * @author ly_smith
 * @Description #TODO  格式化输出 printf
 *
 */
public class Demo05 {
    public static void main(String[] args) {
//        runPrintf();
        ArrayList<Student> list = new ArrayList<>();//创建集合用来存储学生对象
        stuInit(list);//初始化化集合，赋值
        showStuList(list);//显示学生信息
    }

    private static void showStuList(ArrayList<Student> list) {
//        System.out.printf("┌ ┬ ┐├ ┼ ┤└ ┴ ┘ ─ │");
        System.out.printf("┌────────┬────┐\n");
        System.out.printf("│%-8s│%-4s│\n","NAME","AGE");
        for (Student stu : list) {//遍历学生对象
            System.out.printf("├────────┼────┤\n");
            System.out.printf("│%-8s│%-4d│\n",stu.getName(),stu.getAge());
        }
        System.out.printf("└────────┴────┘\n");
    }

    private static void stuInit(ArrayList<Student> list) {
        list.add(new Student("Andy",18));
        list.add(new Student("Jack",22));
        list.add(new Student("Jim",26));
        list.add(new Student("Lucy",31));
        list.add(new Student("Tom",25));
    }

    // 格式化输出基本使用
    private static void runPrintf() {
        System.out.printf("整数类型：%d\n",9527);
        System.out.printf("float类型：%f\n",3.14f);
        System.out.printf("字符串类型：%s\n","String");
        System.out.printf("我的姓名是%s,年龄是%d,身高是%f\n","Andy",21,1.78f);

        //限定输出宽度
        System.out.printf("***********%s***********\n","Hello");
        System.out.printf("***********%10s***********\n","Hello");
        System.out.printf("***********%-10s***********\n","Hello");
    }
}
```

**双色球 - 数组版**

```java
import java.util.Arrays;
import java.util.Random;

/**
 * @author ly_smith
 * @Description #TODO  双色球 - 数组版
 */
public class Demo01 {
    public static void main(String[] args) {
        Random ran = new Random();//创建随机类对象
        int blueBall = ran.nextInt(16) + 1;//生成蓝球

        int[] arr = new int[6];//创建容器，存放红球
        for (int i = 0; i < arr.length; i++) {//通过循环生成6个随机数，用来控制循环次数的
            //红球的范围是1 ~ 33之间随机选一个，但是不能重复
            int n = ran.nextInt(33) + 1; //每循环一次生成一个红球
            arr[i] = n;//将生成的红球存放到数组中

            //做红球去重的处理
            for (int j = 0; j < i; j++) {
                if(n == arr[j]){//用当前生成的红球与数组中存储的红球做对比
                    i--;//外循环需要重新再摇一次红球
                    break;
                }
            }
        }

//        Arrays.sort();
        //冒泡排序法
        for (int i = 0; i < arr.length - 1; i++) {//控制最多循环比较的次数
            for (int j = 0; j < arr.length - 1 - i; j++) {//控制前后两个数比较的次数
                if(arr[j] > arr[j + 1]){
                    int t = arr[j];
                    arr[j] = arr[j +1];
                    arr[j + 1] = t;
                }
            }
        }

        System.out.println("红球：" + Arrays.toString(arr) + " | 蓝球[" + blueBall + "]");

    }
}
```

<a name="bd931175"></a>

## 2 - Collection

<a name="4a7e68b4"></a>

### 2.1 - 概述

> 单列集合的顶层接口，既然是接口就不能直接实例化，需要通过实现类。

<a name="06c62f9f"></a>

### 2.2 - Collection集合常用方法

| 方法名                       | 说明                                     |
| ---------------------------- | ---------------------------------------- |
| `boolean add(E e)`           | 添加元素到集合的末尾，成功则返回true     |
| `boolean remove(Object o)`   | 删除指定元素，成功则返回true             |
| `void clear()`               | 清空集合                                 |
| `boolean contains(Object o)` | 判断参数在集合中是否存在，存储则返回true |
| `boolean isEmpty()`          | 判断集合是否为空，空则返回true           |
| `int size()`                 | 获取集合中元素个数                       |

```java
import java.util.ArrayList;
import java.util.Collection;

/**
 * @author ly_smith
 * @Description #TODO  Collection 常用方法
 */
public class Demo02 {
    public static void main(String[] args) {
        //多态，父类的引用执行子类的对象形成多态
        Collection<String> con = new ArrayList<>();

        con.add("东邪");
        con.add("西毒");
        con.add("南帝");
        con.add("北丐");
        con.add("中神通");

        System.out.println("操作前：" + con);

        //删除元素
        System.out.println(con.remove("中神通"));

        //判断参数是否包含在调用对象中
        System.out.println(con.contains("中神通"));//false
        System.out.println(con.contains("东邪"));//true

        //判断集合是否为空
        System.out.println(con.isEmpty());//false
        con.clear();//清空集合
        System.out.println(con.isEmpty());//true

        System.out.println("操作后：" + con);
    }
}
```

<a name="decdafff"></a>

### 2.3 - Collection集合的遍历

- 迭代器，集合的专属遍历工具

```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

/**
 * @author ly_smith
 * @Description #TODO  Collection集合的遍历
 */
public class Demo03 {
    public static void main(String[] args) {
        Collection<String> con = new ArrayList<>();

        con.add("111");
        con.add("222");
        con.add("333");
        con.add("444");
        con.add("555");

        //没有索引，所以不能使用普通的for循环

        //foreach
        for (String str : con) {
            System.out.print(str + "\t");
        }
        //foreach底层就是使用迭代器实现的，可以通过字节码文件查看验证
        System.out.println();

        //迭代器
        Iterator<String> it = con.iterator();//创建迭代器
        while (it.hasNext()) {//判断下一个位置是否有元素
            System.out.print(it.next() + "\t");
        }
    }
}
```

<a name="075a44fc"></a>

## 3 - List

<a name="a59637b1"></a>

### 3.1 - 概述

- 有序集合（指的是读写顺序，也就是说存储和读取的顺序是一致的，并非逻辑顺序）
- List与Set集合不同，List中允许出现重复的值，Set则不允许。
    <a name="eacf9869"></a>

### 3.2 - List特有方法

| 方法名                     | 说明                 |
| -------------------------- | -------------------- |
| `void add(int index ,E e)` | 根据索引位置添加元素 |
| `E remove(int index)`      | 根据索引位置删除元素 |
| `E set(int index , E e )`  | 根据索引位置设置元素 |
| `E get(int index)`         | 根据索引位置获取元素 |

```java
import java.util.ArrayList;
import java.util.List;

/**
 * @author ly_smith
 * @Description #TODO  List集合特有方法
 */
public class Demo04 {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();

        //追加的方式添加元素
        list.add("111");
        list.add("222");
        list.add("333");
        list.add("444");

        //插入元素
        list.add(2,"000");

        //删除
        System.out.println(list.remove(3));

        //设置元素
        System.out.println(list.set(2,"333"));

        System.out.println(list);

        //List集合可以通过普通for循环遍历
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}
```

<a name="1631b4a3"></a>

### 3.3 - 并发性修改异常

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.ListIterator;

/**
 * @author ly_smith
 * @Description #TODO  并发性修改异常
 */
public class Demo05 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();//创建空集合

        list.add("JavaSE");
        list.add("MySql");
        list.add("Linux");
        list.add("Redis");

        //创建迭代器对象，同时迭代器会对于集合有预期的迭代次数
//        Iterator<String> it = list.iterator();
//        while (it.hasNext()){
//            String str = it.next();
//            if ("MySql".equals(str)){
//                list.add("JDBC");
//            }
//        }//java.util.ConcurrentModificationException  并发性修改异常

        //解决并发性修改异常的方法
        //方法一：不使用迭代器
//        for (int i = 0; i < list.size(); i++) {
//            if("MySql".equals(list.get(i))){
//                list.add(i + i,"JDBC");
//            }
//        }

        //方法二：listIterator,更高级的迭代器
        ListIterator<String> lit = list.listIterator();//可以更改预期迭代的个数
        while (lit.hasNext()){//判断下一个位置是否有元素
            if("MySql".equals(lit.next())){//在集合中找MySql
                lit.add("JDBC");
            }
        }

        System.out.println(list);
    }
}
```

<a name="e347f998"></a>

### 3.4 - 数据结构

> 计算机存储与组织数据的方式，其中包含了若干种特定关系的集合，通常通过精心设计的数据结构会给程序带来效率上的提高

```java
import java.util.ArrayList;

/**
 * @author ly_smith
 * @Description #TODO 数据结构的应用
 */
public class Demo06 {
    public static void main(String[] args) {
        String[] sex = {"girl","boy "};
        ArrayList<Student> stuList = new ArrayList<>();//创建集合用来存储学生对象

        stuList.add(new Student("Andy",1,18));
        stuList.add(new Student("Lily",0,21));
        stuList.add(new Student("Jack",1,16));
        stuList.add(new Student("Lucy",0,19));

        for (Student stu : stuList) {//遍历集合
            System.out.printf("%s\t%s\t%d\n",
                    stu.getName(),
                    sex[stu.getSex()],
                    stu.getAge());
        }
    }
}
```

<a name="735f3186"></a>

#### 3.4.1 - 栈模型

> 栈模型：只有一个开口，先进后出，后进先出

- 栈底元素：最先进入到栈模型的元素
- 栈顶元素：最后进入到栈模型的元素
- 压栈、进栈：数据进入栈模型的过程
- 弹栈、出栈：数据离开栈模型的过程
    <a name="3a75b9ac"></a>

#### 3.4.2 - 队列模型

> 队列模型：有两个开口，先进先出，后进后出，数据的流向只有一个方法

- 入队列：数据从后端进入队列的过程
- 出队列：数据从前端离开队列的过程
    <a name="8a766d4f"></a>

#### 3.4.5 - 数组模型

> 数组模型，是一种查询和修改快，但是删除和添加慢的一种数据模型。

- 查询和修改元素可以通过索引直接定位到元素位置
- 添加和删除元素需要对目标元素之后的内容做位置上的调整
- 交换法排序算法

```java
import javax.swing.*;
import java.util.Arrays;

/**
 * @author ly_smith
 * @Description #TODO  数组中经典算法：交换法排序
 */
public class Demo07 {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5,6,7,8,9,0};//初始化方式创建数组

        System.out.println("排序前：" + Arrays.toString(arr));

        //交换法排序
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - 1 - i; j++) {
                if(arr[j] > arr[arr.length - 1 - i]){
                    arr[j] ^= arr[arr.length - 1 - i];
                    arr[arr.length - 1 - i] ^= arr[j];
                    arr[j] ^= arr[arr.length - 1 - i];
                    /*位运算中的运算符,两变量值的互换
                        a ^= b;
                        b ^= a;
                        a ^= b;
                    * */
                }
            }
        }

        System.out.println("排序后：" + Arrays.toString(arr));
    }
}
```

<a name="4ea7905a"></a>

#### 3.4.6 - 链表模型

> 针对于数组而言，链表模型是一种查询和修改慢，添加和删除快的一种数据模型。

- 节点：一个数据单元
- 数据域：数据单元中用来存储数据的成员
- 指针域：数据单元中用来存储下一个节点成员
    <a name="31ae153a"></a>

#### 3.4.7 - 一维数组模拟栈模型

```java
package demo01;

/**
 * @author ly_smith
 * @Description #TODO  模拟栈模型
 */
public class ArrayStack {
    private int top = -1;//控制栈顶元素位置
    private int maxSize;//设置栈模型大小
    private int[] stack;//用来存放栈的引用类型

    //通过构造器设置栈中存储的最大值
    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[maxSize];
    }
    //满栈
    public boolean isFull(){
        return top == maxSize - 1;
    }
    //空栈
    public boolean isEmpty(){
        return top == -1;
    }
    //压栈（进栈）
    public void push(int n){
        //判断栈是否已满
        if(isFull()){
            throw new RuntimeException("栈已满");
        }
        //栈没满
        top++;
        stack[top] = n;
    }
    //弹栈（出栈）
    public int pop(){
        if(isEmpty()){
            throw new RuntimeException("栈为空");
        }
        //取栈中的数据
        int val = stack[top];
        top--;
        return val;
    }
    //显示栈中的数据
    public void list(){
        if (isEmpty()){
            System.err.println("栈空，没有数据");
            return;
        }
        //从栈顶开始展示数据
        for (int i = top; i >= 0 ; i--) {
            System.out.println("stack[" + i +"] = " + stack[i]);
        }
    }

}
----------------------------------------------------
package demo01;

import java.util.Scanner;

/**
 * @author ly_smith
 * @Description #TODO  栈模型测试类
 */
public class Stack {
    public static void main(String[] args) {
        //通过栈模型的构造器设置最多存储的数据长度，以及创建栈模型对象
        ArrayStack stack = new ArrayStack(10);
        String key = "";//用来存储用户键盘输入的指令
        boolean loop = true;//控制是否退出菜单
        Scanner sc = new Scanner(System.in);

        while (loop){
            System.out.println("show:显示栈");
            System.out.println("exit:退出程序");
            System.out.println("push:添加数据到栈");
            System.out.println("pop:从栈中取出数据");
            System.out.println("请输入你的选项：");
            key = sc.next();//用过键盘输入获取用户的选项

            switch (key){
                case "show"://显示栈
                    stack.list();
                    break;
                case "push"://压栈
                    int val = sc.nextInt();//获取用户想要存储的整数类型
                    stack.push(val);
                    break;
                case "pop"://弹栈
                    try {
                        int res = stack.pop();//从栈顶开始取，栈中可能没有数据，所以会抛出异常
                        System.out.println("出栈的数据为：" + res);
                    }catch (Exception e){
                        System.err.println(e.getMessage());//打印异常原因
                    }
                    break;
                case "exit":
                    sc.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
    }
}
```

<a name="ca5c876c"></a>

### 3.5 - LinkedList

- ArrayList :底层数据结构是可变长数组，特点就是查询和修改效率高，插入和删除效率低
- LinkedList：底层数据结构是链表，特点是查询和修改效率低，但是插入和删除效率高

> 未来的实际应用中，如果增删多，使用LinekdList，如果改查多，使用ArrayList

<a name="15f32f9f"></a>

#### 3.5.1 - LinkedList特有方法

| 方法名                      | 说明             |
| --------------------------- | ---------------- |
| `public void addFirst(E e)` | 在头结点添加成员 |
| `public void addLast(E e)`  | 在尾结点添加成员 |
| `public E getFirst()`       | 获取头结点       |
| `public E getLast()`        | 获取尾结点       |
| `public E removeFirst()`    | 删除头结点       |
| `public E removeLast()`     | 删除尾结点       |

```java
import java.util.LinkedList;

/**
 * @author ly_smith
 * @Description #TODO  LinkedList特有方法
 */
public class Demo02 {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>();//创建链表集合

        list.add("111");
        list.add("222");
        list.add("333");
        list.add("444");

        System.out.println("操作之前：" + list);

        //添加头尾结点成员
        list.addFirst("AAA");
        list.addLast("ZZZ");

        //获取头尾结点
        System.out.println(list.getFirst());
        System.out.println(list.getLast());

        //删除头尾结点
        System.out.println(list.removeFirst());
        System.out.println(list.removeLast());

        System.out.println("操作之后：" + list);

    }
}
```

<a name="d4f4ecae"></a>

## 4 - Set

<a name="7a1efdfa"></a>

### 4.1 - 概述

- Set是一个接口，继承于Collection，与List类似，都需要通过实现类对其进行操作
- 特点 
    - 不允许包含重复的值
    - 没有索引，所以不能使用普通的for循环进行遍历

```java
import java.util.HashSet;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO  Set集合基本应用
 */
public class Demo03 {
    public static void main(String[] args) {
        //父类的引用指向子类对象，形成多态
        Set<String> set = new HashSet<>();

        set.add("黄固");
        set.add("欧阳锋");
        set.add("段智兴");
        set.add("洪七公");

        set.add("段智兴");

        System.out.println(set);//打印集合
        //HashSet集合对于数据的读写顺序不做保证
        //相同的元素，多次存储，只能被保留一个，而且不报错
        //List集合可以存储重复的值，但是Set集合不可以
    }
}
```

<a name="cf7c4f20"></a>

### 4.2 - 哈希值

> 哈希值就是JDK根据对象的地址或者字符串或者数值通过自己内部的算法计算出来的一个整数类型数据
> `public int hashCode()`  用来获取哈希值，来自于Object顶层类

- 对象的哈希值的特点 
    - 同一个对象多次调用`hashCode()`方法，得到的返回值是相同的
    - 默认情况下，不同的对象哈希码值也是不同的（特殊情况）

```java
/**
 * @author ly_smith
 * @Description #TODO  对象的哈希值特点
 */
public class Demo04 {
    public static void main(String[] args) {
        //对象相同，哈希码值相同
        System.out.println("Andy".hashCode());
        System.out.println("Andy".hashCode());

        //对象不同，哈希码值不同，但是特殊情况除外
        System.out.println(new Object().hashCode());
        System.out.println(new Object().hashCode());

        //对象不同，但是哈希码值也有可能会相同，如果单单只凭借哈希码值判断对象是否相同，是不严谨的。
        System.out.println("迯鄒".hashCode());
        System.out.println("述郳".hashCode());
        System.out.println("迱郔".hashCode());
        System.out.println("迲邵".hashCode());
        System.out.println("迳邖".hashCode());
    }
}
```

<a name="b7098ee3"></a>

### 4.3 - HashSet的去重原理

```java
import java.util.Objects;

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

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age &&
                Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
------------------------------------------------
import java.util.HashSet;

/**
 * @author ly_smith
 * @Description #TODO  HashSet去重原理
 */
public class Demo05 {
    public static void main(String[] args) {
        //创建HashSet集合
        HashSet<Student> set = new HashSet<>();

        set.add(new Student("黄固",35));
        set.add(new Student("欧阳锋",28));
        set.add(new Student("段智兴",37));
        set.add(new Student("洪七公",29));
        set.add(new Student("段智兴",37));//   ?

        //从程序底层角度出发，两个段智兴不是同一个对象，都有自己的存储空间，所以哈希码值不同
        //在学生实体类内部重写equals和hashCode方法，就可以实现对象的去重操作
        for (Student stu : set) {
            System.out.println(stu);
        }
    }
}
```

- **去重原理：** 
    - 先对比hashCode方法，如果哈希值相同，再去比较equals的内容，这两个方法经常需要在自定义类中进行重写，帮助我们解决逻辑上的去重关系
- **HashSet集合的特点** 
    - 底层数据结构是“哈希表”
    - 集合对于读写顺序不做保证
    - 没有索引
    - Set集合中的内容不能重复
        <a name="840244f3"></a>

### 4.4 - 哈希表

> 通过  数组  +  链表  实现的一种数结构
> 哈希表的构造方法参数是一个长度为16个元素的数组，通过（对象的哈希值 % 16）的值，作为头结点在数组中选择相对应的位置。

<a name="7247e547"></a>

### 4.5 - LinkedHashSet

- 特点 
    - LinkedHashSet是哈希表和链表实现的set接口，具有可预测的读写顺序
    - 由链表来保证元素的有序
    - 由哈希表来保证元素的唯一性

```java
import java.util.LinkedHashSet;

/**
 * @author ly_smith
 * @Description #TODO  LinkedHashSet集合
 */
public class Demo06 {
    public static void main(String[] args) {
        LinkedHashSet<String> set = new LinkedHashSet<>();

        set.add("黄固");
        set.add("欧阳锋");
        set.add("段智兴");
        set.add("洪七公");

        set.add("段智兴");

        System.out.println(set);
        //因为数据结构越复杂程序效率越低
    }
}
```

<a name="a1ab6739"></a>

### 4.6 - TreeSet

- 特点 
    - 有序元素的存储集合，这里的顺序指的是逻辑顺序 
        - `TreeSet()`：自然规则排序，默认规则
        - `TreeSet(Comparator com)`：自定义规则排序

```java
import java.util.Comparator;
import java.util.TreeSet;

/**
 * @author ly_smith
 * @Description #TODO  TreeSet
 */
public class Demo07 {
    public static void main(String[] args) {
//        TreeSet<Integer> set = new TreeSet<>();//默认排序规则，升序排序
        TreeSet<Integer> set = new TreeSet<>(new Comparator<Integer>() {
            @Override  //compare方法的返回值决定了排序规则
            public int compare(Integer o1, Integer o2) {
//                return 0; //等于0  表示后面插入的元素与当前元素相等
//                return 9527;//大于0  表示后面插入的元素比当前元素大
//                return -1314; //小于0  表示后面插入的元素比当前元素小
//                return o1 - o2;//代表升序排序，默认规则
                return o2 - o1; //代表降序排序
            }
        });

        set.add(66);
        set.add(22);
        set.add(33);
        set.add(11);
        set.add(55);
        set.add(44);

        System.out.println(set);
    }
}
```

- 自定义排序，存储自定义对象
    <a name="5a5cf0cb"></a>

## 5 - 泛型

> 编译时的类型安全检测机制，也可以把这个数据类型理解成是一种可以传递的参数
> 泛型类、泛型方法、泛型接口

<a name="a6e1c439"></a>

### 5.1 - 泛型的定义格式

- <类型> ： 表示只是一种类型的格式
- <类型1，类型2，类型3...>:表示的是多种类型的格式，用逗号分割
    <a name="63f313af"></a>

### 5.2 - 泛型类

```java
package demo02;

/**
 * @author ly_smith
 * @Description #TODO  泛型类
 * E - Element
 * K - Key
 * T - Type
 * V - Value
 */
public class Generic<T> {
    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }
}
-------------------------------
package demo02;

/**
 * @author ly_smith
 * @Description #TODO  泛型类的测试类
 */
public class Demo02 {
    public static void main(String[] args) {
        Generic<String> strG = new Generic<>();
        strG.setT("我是一个字符串");
        System.out.println(strG.getT());

        Generic<Integer> intG = new Generic<>();
        intG.setT(9527);
        System.out.println(intG.getT());

        Generic<Student> stuG = new Generic<>();
        stuG.setT(new Student("Andy",18));
        System.out.println(stuG.getT());

        //泛型就会以参数的方式传递到泛型类中
    }
}
```

<a name="f608438b"></a>

### 5.3 - 泛型方法

```java
package demo03;

/**
 * @author ly_smith
 * @Description #TODO  泛型方法
 */
public class Generic {
    //定义泛型方法
    public <T> void show(T i) {
        System.out.println(i);
    }

    /*public void show(String string) {
        System.out.println(string);
    }

    public void show(float v) {
        System.out.println(v);
    }

    public void show(double v) {
        System.out.println(v);
    }*/
}
-----------------------------------------------------
package demo03;

/**
 * @author ly_smith
 * @Description #TODO  泛型方法测试类
 */
public class Demo03 {
    public static void main(String[] args) {
        Generic g = new Generic();

        g.show(123);
        g.show("String");
        g.show(3.14f);
        g.show(3.14);
    }
}
```

<a name="f24a149e"></a>

### 5.4 - 泛型接口

```java
package demo04;

/**
 * @author ly_smith
 * @Description #TODO  泛型接口
 */
public interface Generic<T> {
    void show(T t);//定义抽象方法
}
-------------------------------------------------------------
  package demo04;

/**
 * @author ly_smith
 * @Description #TODO
 */
public class GenericImpl<T> implements Generic<T>{
    @Override
    public void show(T t) {
        System.out.println("实现类中的show方法:" + t);
    }
package demo04;
-------------------------------------------------------------
package demo04;

/**
 * @author ly_smith
 * @Description #TODO  泛型接口测试类
 */
public class GenericTest {
    public static void main(String[] args) {
        GenericImpl<String> strG = new GenericImpl<>();
        strG.show("String");


        //通过匿名内部类的对象直接调用并实现其中的方法
        new Generic<Integer>() {
            @Override
            public void show(Integer integer) {
                System.out.println("匿名内部类的show方法" + integer);
            }
        }.show(9527);
    }
}
```

<a name="ee264d8b"></a>

### 5.5 - 泛型通配符

```java
import java.util.ArrayList;

/**
 * @author ly_smith
 * @Description #TODO  泛型通配符
 */
public class Demo05 {
    public static void main(String[] args) {
        // <?>  通配到所有 相当于Object
        ArrayList<?> list01 = new ArrayList<Object>();
        ArrayList<?> list02 = new ArrayList<Integer>();
        ArrayList<?> list03 = new ArrayList<Float>();
        ArrayList<?> list04 = new ArrayList<Number>();

        // <? extends Number>  上限
        ArrayList<? extends Number> list05 = new ArrayList<Number>();
        ArrayList<? extends Number> list06 = new ArrayList<Integer>();
        ArrayList<? extends Number> list07 = new ArrayList<Float>();
        //因为String类型与Number类型没有继承关系
        //ArrayList<? extends Number> list08 = new ArrayList<String>();

        //   <? super Number>  下限
        ArrayList<? super Number> list09 = new ArrayList<Number>();
//        ArrayList<? super Number> list10 = new ArrayList<Integer>();
//        ArrayList<? super Number> list11 = new ArrayList<Float>();
        ArrayList<? super Number> list12 = new ArrayList<Object>();
    }
}
```

<a name="253fb396"></a>

### 5.6 - 泛型通配符实际应用

```java
import java.util.ArrayList;

/**
 * @author ly_smith
 * @Description #TODO  泛型通配符的实际应用
 */
public class Demo06 {
    public static void main(String[] args) {
        ArrayList<String> strList = new ArrayList<>();
        show(strList);
//        showUp(strList); //与Number无关 所以放不进去
//        showDown(strList);//与Number无关 所以放不进去
        ArrayList<Integer> intList = new ArrayList<>();
        show(intList);
        showUp(intList);
//        showDown(intList);
        ArrayList<Number> numList = new ArrayList<>();
        show(numList);
        showUp(numList);
        showDown(numList);
        ArrayList<Object> objList = new ArrayList<>();
        show(objList);
//        showUp(objList);  Object是Number的父类，需要的是Number的子类
        showDown(objList);
    }
    // <?>  通配所有
    private static void show(ArrayList<?> list) {}

    // <? extends Number>  表示上限，传递的类型必须是Number本身或其子类
    private static void showUp(ArrayList<? extends Number> list) {}

    // <? extends Number>  表示下限，传递的类型必须是Number本身获取其父类
    private static void showDown(ArrayList<? super Number> list) {}
}
```

<a name="38be3559"></a>

## 6 - Map

> 双列集合：用来存储键值对的集合

<a name="26f942e0"></a>

### 6.1 - 概述

- `interface Map<K,V>`：K(key) 键    V(value)值
- 将键映射到值的对象，不能出现重复的键，每个键最多映射到一个值

例子：学生学号和姓名

| 学号（key） | 姓名（value） |
| ----------- | ------------- |
| STU001      | 张三          |
| STU002      | 李四          |
| STU003      | 张三          |

```java
import java.util.HashMap;
import java.util.Map;

/**
 * @author ly_smith
 * @Description #TODO  Map集合基本使用
 */
public class Demo07 {
    public static void main(String[] args) {
        //父类引用指向子类对象形成多态
        Map<String,String> map = new HashMap<>();

        //设置值
        map.put("STU001","Andy");
        map.put("STU002","Jack");
        map.put("STU003","Lucy");
        map.put("STU004","Lily");
        map.put("STU005","Bob");
        //如果键重复，则代表设置元素
        //如果键不重复，则代表添加元素

        System.out.println(map);
    }
}
```

<a name="d5ae073a"></a>

### 6.2 - Map的集合功能

| 方法名                    | 说明               |
| ------------------------- | ------------------ |
| `V put(K key,V value)`    | 设置键值对         |
| `V remove(Object key)`    | 删除元素           |
| `void clear()`            | 清空集合           |
| `boolean containsKey()`   | 判断键是否存在     |
| `boolean containsValue()` | 判断值是否存在     |
| `boolean isEmpty()`       | 判断是否为空       |
| `int size()`              | 获取集合中元素个数 |


```java
import java.util.HashMap;
import java.util.Map;

/**
 * @author ly_smith
 * @Description #TODO  Map集合的基本功能
 */
public class Demo08 {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<>();

        //添加元素
        map.put("STU001","Andy");
        map.put("STU002","Jack");
        map.put("STU003","Lucy");
        map.put("STU004","Lily");
        map.put("STU005","Bob");

        //删除元素
        System.out.println(map.remove("STU003"));

        //判断键和值是否存在
        System.out.println(map.containsKey("STU003"));//false
        System.out.println(map.containsKey("STU002"));//true
        System.out.println("---------------" );
        System.out.println(map.containsValue("Lucy"));//false
        System.out.println(map.containsValue("Bob"));//true

        System.out.println(map);
    }
}
```

<a name="77f25833"></a>

### 6.3 - Map集合的获取功能

| 方法名                           | 说明                       |
| -------------------------------- | -------------------------- |
| `V get(Object key)`              | 根据键获取值               |
| `Set<K> keySet()`                | 获取所有键的Set集合        |
| `Collection<V> values()`         | 获取所有值的Collection集合 |
| `Set<Map.Entry<K,V>> entrySet()` | 获取所有键值对的对象的集合 |


```java
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO  Map集合的获取功能
 */
public class Demo09 {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<>();

        //添加元素
        map.put("STU001","Andy");
        map.put("STU002","Jack");
        map.put("STU003","Lucy");
        map.put("STU004","Lily");
        map.put("STU005","Bob");

        //get通过键获取值
        System.out.println(map.get("STU005"));
        System.out.println(map.get("STU001"));

        //keySet
        Set<String> keySet = map.keySet();
        System.out.println(keySet);

        //values
        Collection<String> values = map.values();
        System.out.println(values);

        //entrySet
        Set<Map.Entry<String, String>> es = map.entrySet();
        System.out.println(es);
    }
}
```

<a name="f4c51c74"></a>

### 6.4 - Map集合的遍历

```java
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO  Map集合的遍历
 */
public class Demo10 {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<>();

        //添加元素
        map.put("STU001","Andy");
        map.put("STU002","Jack");
        map.put("STU003","Lucy");
        map.put("STU004","Lily");
        map.put("STU005","Bob");

        //遍历方式一  values
        Collection<String> values = map.values();//获取全部值的集合
        for (String str : values) {//遍历集合
            System.out.println(str);
        }

        //遍历方式二：keySet
        Set<String> keySet = map.keySet();//获取所有键，存放到Set集合中
        for (String key : keySet) {//遍历键
            System.out.println(key + "<==>" + map.get(key));
        }

        System.out.println("-----------------------");

        //遍历方式三： entrySet
        Set<Map.Entry<String, String>> es = map.entrySet();//获取所有键值对的对象，存放到集合中
        for (Map.Entry<String, String> e: es) {//遍历集合中的么每个对象，每个对象都是键值对对象
            String key = e.getKey();
            String value = e.getValue();
            System.out.println(key + "<==>" + value);
        }
    }
}
```

<a name="44827622"></a>

### 6.5 - 存储并遍历学生对象

```java
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO Map集合存储并遍历学生对象
 */
public class Demo11 {
    public static void main(String[] args) {
        HashMap<String, Student> hm = new HashMap<>();//创建双列集合用来存储学生对象

        //添加元素
        hm.put("STU001",new Student("Andy",18));
        hm.put("STU002",new Student("Jack",24));
        hm.put("STU003",new Student("Lucy",27));
        hm.put("STU004",new Student("Lily",21));

        showMap01(hm);
        showMap02(hm);
        showMap03(hm);

    }

    private static void showMap03(HashMap<String,Student> hm) {
        Set<Map.Entry<String, Student>> es = hm.entrySet();//取出对键值对的对象
        for (Map.Entry<String, Student> e : es) {//遍历每个键值对对象
            System.out.println(e.getKey() + "<==>" + e.getValue().getName() + "-->" + e.getValue().getAge());
        }
    }

    private static void showMap02(HashMap<String,Student> hm) {
        Set<String> keySet = hm.keySet();//取出键的Set集合
        for (String key : keySet) {//遍历每个学生的学号
            Student stu = hm.get(key);//通过学号取出学生对象
            System.out.println(key + "<==>" + stu.getName() + "-->" + stu.getAge());
        }
    }

    private static void showMap01(HashMap<String,Student> hm) {
        Collection<Student> values = hm.values();
        for (Student stu : values) {
            System.out.println(stu.getName() + "-->" + stu.getAge());
        }
    }
}
```

<a name="83d9596b"></a>

### 6.6 - 集合的嵌套使用

- ArrayList集合中存储HashMap集合

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO  集合的嵌套使用  ArrayList集合中存储HashMap集合
 */
public class Demo01 {
    public static void main(String[] args) {
        ArrayList<HashMap<String, String>> arrList = new ArrayList<>();//创建ArrayList集合

        HashMap<String, String> hm01 = new HashMap<>();
        HashMap<String, String> hm02 = new HashMap<>();
        HashMap<String, String> hm03 = new HashMap<>();

        //双列集合中设置元素
        hm01.put("郭德纲","于谦");
        hm01.put("岳云鹏","孙越");
        hm01.put("牛群","冯巩");

        hm02.put("王宝强","马蓉");
        hm02.put("文章","马伊琍");
        hm02.put("许凯威","杨幂");

        hm03.put("雷公","电母");
        hm03.put("玉皇大帝","王母娘娘");
        hm03.put("土地公","土地婆");

        //将双列集合存储到ArrayList集合中
        arrList.add(hm01);
        arrList.add(hm02);
        arrList.add(hm03);

        //遍历集合
        showList01(arrList);//keySet
        System.out.println("---------------------");
        showList02(arrList);//entrySet
    }

    private static void showList02(ArrayList<HashMap<String, String>> arrList) {
        for (HashMap<String, String> hm : arrList) {//遍历外层单列集合，取出每个双列集合对象
            Set<Map.Entry<String, String>> es = hm.entrySet();//取出每个集合的每对键值对对象存放到set集合中
                for (Map.Entry<String, String> e : es) {//遍历每个双列集合中的每对键值对对象
                    String key = e.getKey();
                    String value = e.getValue();
                    System.out.println(key + "---" + value);
                }
        }
    }

    private static void showList01(ArrayList<HashMap<String, String>> arrList) {
        for (HashMap<String, String> hm : arrList) {//遍历外层单列集合，取出每个双列集合对象
            Set<String> keySet = hm.keySet();//取出每个双列集合的键的Set集合
            for (String key : keySet) {//取出每个集合的键
                String value = hm.get(key);//通过键获取值
                System.out.println(key + "---" + value);
            }
        }
    }
}
```

<a name="44d5ca25"></a>

### 6.7 - TreeMap

```java
import java.util.TreeMap;

/**
 * @author ly_smith
 * @Description #TODO  TreeMap
 */
public class Demo02 {
    public static void main(String[] args) {
        /*TreeMap<String, String> map = new TreeMap<>();//创建TreeMap集合

        map.put("Andy","广东");
        map.put("Jack","香港");
        map.put("Tom","厦门");
        map.put("Bob","上海");
        map.put("Lucy","北京");

        //当键为字母时，按照字典顺序进行排序
        System.out.println(map);*/

        TreeMap<Integer, String> map = new TreeMap<>();

        map.put(4,"广东");
        map.put(2,"香港");
        map.put(1,"厦门");
        map.put(5,"上海");
        map.put(3,"北京");

        //当键为数字时，按照键的升序方式排序
        System.out.println(map);
    }
}
```

<a name="2973730d"></a>

### 6.8 - 案例

- 统计字符串中字符出现的数量

```java
import java.util.HashMap;
import java.util.Scanner;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO  统计字符串中字符出现的数量   Hello
 */
public class Demo03 {
    public static void main(String[] args) {
        System.out.println("请输入一个字符串：");
        String line = new Scanner(System.in).nextLine();//获取用户的键盘输入的字符串

        //使用键存储字符串中每个字符    使用值存储字符出现的数量
        HashMap<Character, Integer> map = new HashMap<>();

        for (int i = 0; i < line.length(); i++) {
            char key = line.charAt(i);//将字符作为键
            Integer count = map.get(key);//去Map集合中找键所对应的值（数量）
            if(count == null){//如果键不存在，则会返回null类型，表示该字符没有出现过，是第一次出现
                map.put(key,1);//key在集合中没有出现过，则赋值初始值为1，表示第一次出现
            }else{
                map.put(key,++count);
            }
        }

        //遍历集合
        Set<Character> keySet = map.keySet();//获取键的Set集合
        for (Character key : keySet) {
            System.out.println(key + ":" +map.get(key) + "个");
        }

    }
}
```

<a name="4630737a"></a>

## 7 - Properties

> Properties类位于java.util工具类中，是Java配置文件所使用的类


```java
import java.util.Properties;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO  Properties
 */
public class Demo04 {
    public static void main(String[] args) {
        Properties prop = new Properties();//创建配置文件对象

        //设置配置文件信息
        prop.setProperty("STU001","黄固");
        prop.setProperty("STU002","欧阳锋");
        prop.setProperty("STU003","段智兴");
        prop.setProperty("STU004","洪七公");

        //遍历方式
        Set<String> keySet = prop.stringPropertyNames();//与Map集合中keySet方法相同
        for (String key : keySet) {
            System.out.println(key + "<==>" + prop.getProperty(key));
        }
    }
}
```

- 加载和存储配置文件信息

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Properties;
import java.util.Set;

/**
 * @author ly_smith
 * @Description #TODO  Properties加载和存储配置文件信息
 */
public class Demo05 {
    public static void main(String[] args) throws IOException {
        Properties prop = new Properties();//构造配置文件类的对象

//        mySave(prop,"./myConf.ini");
        myLoad(prop,"./myConf.ini");
    }

    /**
     * 将配置文件中的信息加载到内存中来
     * @param prop
     * @param src
     */
    private static void myLoad(Properties prop, String src) throws IOException {
        FileReader fr = new FileReader(src);//构造字符输入流读数据
        prop.load(fr);
        fr.close();

        //遍历配置信息
        Set<String> keySet = prop.stringPropertyNames();//取出键的Set集合
        for (String key : keySet) {//遍历每个键
            System.out.println(key + "<==>" + prop.getProperty(key));
        }
    }
  
    private static void mySave(Properties prop, String src) throws IOException {
        prop.setProperty("USERNAME","root");
        prop.setProperty("PASSWORD","123456");
        prop.setProperty("DATABASE","YX2209");
        prop.setProperty("PORT","3306");

        FileWriter fw = new FileWriter(src);//构造字符流对象，与文件相关联
        prop.store(fw,"MyDataBase Configure!~");//将prop对象内的配置文件信息通过流写入到文件中
        fw.close();
    }
}
```
