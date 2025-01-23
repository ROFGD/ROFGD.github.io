---
title: JavaSe基础---集合02
date: 2020-05-15 20:42:10
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

#### 一、集合：

-   1.1 什么是集合，有什么用？
    -   集合实际上是一个容器，可以容纳其他类型的数据。
    -   数组就是一个集合。

-   1.2 集合不能直接存储基本数据类型，另外集合也不能直接存储java对象，集合当中存储的都是java对象的内存地址。

-   **注：集合在****java****中本身是一个容器，是一个对象**
-   **集合中任何时候存储的都是****"****引用****"**

-   1.3 在Java中每一个不同的集合底层会对应不同的数据结构，往不同的集合中存储元素等于将数据放到了不同的数据结构中使用不同的集合等于使用了不同的数据结构

-   1.4 集合在java.util.*;包下

-   1.5 在Java中集合分为两大类：

    -   一类是单个方式存储元素，这一类集合中超级父接口java.util.collection

    -   一类是以key和valuer的方式存储元素，这一类集合中超级父接口java.util.Map;

#### 二、关于java.util.collection接口中常用的方法：

-   1.collection中能存放什么元素？
    -   没有用"泛型"前，collection中可以存储object中所有子类型，使用"泛型"后，collection中只能存储某个具体的类型，但是不能存储基本数据类型和java对象。
-   2.collection中常用的方法
    -   boolean add(Object e)----向集合中添加元素
    -   int size()-----获取集合中元素的个数
    -   void clear()-----清空集合
    -   boolean contains()----判断集合中是否包含某个元素
    -   boolean remove(Object o)----删除集合中的某个元素
    -   boolean isEmpty()-----判断该集合是否为空
    -   Object[] toArray----调用这个方法可以把集合转化为数组

```java
package 集合;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionText01 {
    public static void main(String[] args) {
        //Collection是一个接口无法实例化，所以使用多态，父类型引用指向子类型对象
        Collection c = new ArrayList();
        //向集合中添加元素
        c.add(1200);
        c.add(3.14);
        c.add(true);
        c.add("Hello World");

        //获取集合中元素的个数
        System.out.println("该集合元素的个数为：" + c.size());

        // 清空集合
        // c.clear();
        // System.out.println("该集合元素个数为：" + c.size());

        //判断该集合中是否包含某个元素
        boolean b = c.contains(1200);
        System.out.println(b);

        boolean b2 = c.contains("helllo");
        System.out.println(b2);

        //删除集合中某个元素
        boolean c1 = c.remove(1200);
        System.out.println(c);

        c.add(1200);
        c.add("马克");

        //判断该集合是否为空
        boolean c2 = c.isEmpty();
        System.out.println(c2);
    }
}

```

#### 三、集合原理：

-   collection中的迭代方式只能在collection以及子类中使用
-   对集合进行遍历/迭代：
    -   第一步：获取集合对象的迭代器对象Iterator
    -   第二步：通过以上获取迭代器对象开始迭代/遍历
        -   boolean hasNext()----如果有元素可以迭代则返回True
        -   Object next()----返回迭代下一个元素

```java
package 集合;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class itertionText {
    public static void main(String[] args) {
        //创建集合对象
        Collection c = new ArrayList();
        //给集合中添加元素
        c.add("hello World");
        c.add(1234);
        c.add(3.14);
        c.add(true);
        c.add("浩克");
        c.add(3);

        //第一步先获取迭代器对象开始迭代/遍历集合
        Iterator it = c.iterator();

        //通过以上获取的迭代器对象开始遍历/迭代
        while(it.hasNext()){
            Object obj = it.next();  //这个方法让迭代器前进一位获取元素
            System.out.println(obj);
        }
    }
}

```

#### 四、深入Collection中的contains方法

-   1.boolean contains(Object o) 如果包含返回true，如果不包含返回false
-   2.contains 方法是用来判断集合中是否包含某个元素的方法，底层调用equals方法进行对比，返回true表示包含这个元素
-   3.存放在集合中的类型，一定要重写equals方法

#### 五、关于集合元素的remove

-   1.当集合的结构发生改变时，必须重新获取迭代器否则会爆出异常"java.util.ConcurrentModificationExeption"
-   2.在迭代集合元素过程中不能调用集合对象的remove方法，删除元素否侧会报异常
-   3.迭代器删除时会自动更新迭代器，并且更新集合，出现“java.util.ConcurrentModificationExeption”是因为集合当中元素删除了但是没有更新迭代器
-   4.在迭代器元素的过程中，一定要使用迭代器Iterator的remove方法，删除元素，不要使用集合自带的remove方法删除元素

#### 六、关于ArraysList集合

-   1.ArrayList集合初始化容量为10，扩容倍数为1.5倍，底层先创建一个长度为0的数组，当添加第一个元素时候，初始容量为10。

-   2.底层是一个数组数据结构

-   3.构造方法：

    -   New ArrayList()
    -   New ArrayList(20)

-   4.如何优化

    -   尽可能减少扩容，因为数组扩容效率比较低，使用该集合时候预估元素个数，给定一个初始化容量减少扩容次数

-   5.数组的优缺点：

    -   优点：检索效率高
    -   缺点：随机增删元素效率比较低，但是向末尾添加元素效率还是很高

    ```java
    package 集合;
    
    import java.util.*;
    
    /*
     * List接口中常用的方法
     *   void add(int index,Object element)
     *   Object get(int index)
     *   int indexOf(Object o)
     *   Object remove(int index)
     *   Object set(int index,object element)
     * */
    
    public class ListText01 {
        public static void main(String[] args) {
            List myList = new ArrayList();
    
            myList.add("C");
            myList.add("D");
            myList.add("C");
            myList.add("D");
            myList.add("C");
            myList.add("A");
            myList.add("B");
            myList.add("A");
            myList.add("B");
            myList.add("C");
    
            //一般不使用，对于ArrayList集合来说处理大量数据时此方法效率低
            //根据元素下标向集合添加元素
            myList.add(2,"King");
            myList.add(3,"Jack");
            myList.add(5,"Make");
    
            Iterator obj = myList.iterator();
            while(obj.hasNext()){
                Object jet = obj.next();
                System.out.println(jet);     //顺序：A B King Jack C Make D
            }
    
            System.out.println("==============================================");
    
            //根据下标获取元素
            Object firstList = myList.get(3);
            System.out.println(firstList);
    
            System.out.println("==============================================");
    
            //根据元素下标使用for循环进行遍历
            for (int i = 0;i < myList.size();i++){
                Object objFirst = myList.get(i);
                System.out.println(objFirst);
            }
    
            System.out.println("==============================================");
    
            //获取元素第一次和最后一次出现的索引
            System.out.println(myList.indexOf("B"));
            System.out.println(myList.lastIndexOf("A"));
    
            System.out.println("==============================================");
    
            //根据下标删除元素
            myList.remove(4);   //删除下标为4的元素
            System.out.println(myList.size());
    
            System.out.println("==============================================");
    
            //修改指定下标的元素
            myList.set(4,"9");
            myList.set(3,"Tom");
            myList.set(5,"3.1345");
            myList.set(6,"Home");
            for (int i = 0; i < myList.size(); i++) {
                Object o = myList.get(i);
                System.out.println(o);
            }
        }
    }
    
    ```

#### 七、单向链表数据结构和Vector集合

-   1.对于链表数据结构来说基本单元是节点(Node)
-   2.每一个节点Node都有两个属性，一个属性是存储数据，另一个是下一个节点的内存地址
-   3.链表的优点：
    -   由于链表上的元素在空间存储上内存地址不连续，所以随机增删元素的时候不会有大量元素位移，因此随机增删元素效率较高，在以后开发中如果遇到随机增删元素的业务比较多时建议使用LinkedList集合
-   4.链表的缺点：
    -   不能通过数学表达式计算被查找元素的内存地址，每一次查找都是从头节点开始遍历，直到找到为止，所以遍历/检索效率较低 
-   关于Vector集合：
    -   1.底层也是一个数组
    -   2.初始容量为10
    -   3.扩容之后是原容量的2倍
    -   4.vector中所有的方法都是同步的，都带有synchronized关键字，是线程安全的，效率较低，使用较少

#### 八、泛型

-   1.JDK5.0之后推出的新特性：泛型
-   2.泛型这种语法机制只在程序编译阶段起作用只是给编译器参考的(运行阶段泛型没用)
-   3.使用泛型的好处：
    -   集合存储元素类型统一
    -   从集合中取出的元素类型是泛型指定的类型，不需要大量“向下转型”
-   4.泛型的缺点：
    -   导致集合中存储元素缺乏多样性
-   5.在JDK8以后引入了自动类型推断机制（钻石表达式）

#### 九、Java.util.Map接口中常用方法

-   1.Map和Collection没有继承关系

-    2.Map集合以key和value方式存储数据：键值对儿

    -   key和value都是引用数据类型
    -   key和value都是存储对象的内存地址
    -   key起主导地位，value是key的一个附属品

-    3.常用方法：

    -   void clear()---->清空集合
    -    boolean containsKey(Object key)---->判断集合中是否包含key
    -   boolean containsValue(Object value)---->判断集合中是否包含values
    -   Set<Map.Entry<K,V>> entrySet()---->将Map集合转为Set集合
    -   V get(Object key)---->通过key获取value
    -    boolean isEmpty()---->判断集合中元素个数是否为0
    -   Set<K> keySet()---->获取集合中所有的key
    -    void put(K key , V value)---->添加数据
    -   V remove(Object key)----通过key删除键值对儿
    -   int size()---->获取键值对儿个数
    -   Collection<V> values()---->获取所有value，返回一个collection

    ```java
    package 集合;
    
    import java.util.Collection;
    import java.util.HashMap;
    import java.util.Map;
    import java.util.Set;
    
    public class MapText01 {
        public static void main(String[] args) {
            //创建Map集合
            Map<Integer,String> student = new HashMap<Integer, String>();
    
            //添加数据
            student.put(1,"A");
            student.put(2,"B");
            student.put(3,"C");
            student.put(4,"D");
            student.put(5,"E");
            student.put(6,"F");
            student.put(7,"G");
            student.put(8,"H");
            student.put(9,"I");
            student.put(10,"J");
            student.put(11,"K");
            student.put(12,"L");
            student.put(13,"M");
            student.put(14,"N");
            student.put(15,"O");
            student.put(16,"P");
    
            //判断集合中是否包含key/values
            Boolean t = student.containsKey(3);
            System.out.println(t);      //true
            Boolean b = student.containsValue("Z");
            System.out.println(b);      //false
    
            System.out.println("========================================");
    
            //通过key获取values
            Object obj = student.get(5);
            System.out.println(obj);        //E
    
            System.out.println("========================================");
    
            //判断集合中元素个数是否为0
            Boolean b1 = student.isEmpty();
            System.out.println(b1);     //false
    
            System.out.println("========================================");
    
            //获取集合中所有的key
            Set<Integer> key = student.keySet();
            for(Integer keys : key){
                System.out.println(keys);
            }  //[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]
    
            System.out.println("========================================");
    
            //通过key删除键值对儿
            //Object obj1 = student.remove(6);
            //System.out.println(obj1);
    
            System.out.println("========================================");
    
            //获取键值对儿个数
            int i = student.size();
            System.out.println(i);      //16
    
            System.out.println("========================================");
    
            //获取所有的values,返回一个collection
            Collection<String> colle = student.values();
            System.out.println(colle); //[A, B, C, D, E, F, G, H, I, J, K, L, M, N, O, P]
    
            System.out.println("========================================");
    
            //将Map集合转换为Set集合
            Set<Map.Entry<Integer,String>> s1 = student.entrySet();
            System.out.println(s1);
            //[1=A, 2=B, 3=C, 4=D, 5=E, 6=F, 7=G, 8=H, 9=I, 10=J, 11=K, 12=L, 13=M, 14=N, 15=O, 16=P]
    
        }
    }
    
    ```

#### 十、遍历Map集合的两种方式（一）

```java
package 集合;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

/*
* 如何遍历Map集合？
*   第一种方式:先循环遍历拿到key,通过key找到对象value
* */

public class MapText02 {
    public static void main(String[] args) {
        //创建Map集合
        Map<Integer,String> map = new HashMap<Integer, String>();

        //向集合中添加数据
        map.put(1,"张三");
        map.put(2,"李四");
        map.put(3,"王五");
        map.put(4,"赵六");
        map.put(5,"Make");
        map.put(6,"Tom");
        map.put(7,"Rocky");

        //遍历Map集合
        //第一种方式:先循环遍历拿到key,通过key找到对象value
        //迭代器
        Set<Integer> keys = map.keySet(); //使用"V get(Object key)---->通过key获取value"

        Iterator<Integer> it = keys.iterator(); //获取迭代器
        while (it.hasNext()){
            Integer key = it.next();
            String value = map.get(key);
            System.out.println(key + "---->" + value);
        }

        System.out.println("=================================");

        //foreach
        for(Integer i : keys){
            System.out.println(i + "---->" + map.get(i));
        }

    }
}

```

#### 十一、遍历Map集合两种方式(二)

```java
package 集合;
import java.util.*;
/*
 * 如何遍历Map集合？
 *   1.第一种方式:先循环遍历拿到key,通过key找到对象value
 *     第二种方式:通过把Map转换为Set集合"Set<Map.Entry<K,V>> entrySet()---->将Map集合转为Set集合"
 *   2.建议使用第二种方式的for增强遍历，因为效率高，适合在大量数据环境下使用
 * */
public class MapText03 {
    public static void main(String[] args) {
        //创建Map集合对象
        Map<Integer, String> map = new HashMap<Integer, String>();
        //集合中添加元素
        map.put(1, "小米");
        map.put(2, "华为");
        map.put(3, "OPPO");
        map.put(4, "VIVO");
        map.put(5, "三星");
        map.put(6, "一加");
        map.put(7, "魅族");
        //先调用entrySet()方法，在使用迭代器
        Set<Map.Entry<Integer, String>> it = map.entrySet();
        //获取迭代器
        Iterator<Map.Entry<Integer, String>> it2 = it.iterator();
        while (it2.hasNext()) {
            //数据类型 变量名 = 引用.next();
            Map.Entry<Integer, String> node = it2.next();
            Integer key = node.getKey();
            String value = node.getValue();
            System.out.println(key + "==" + value);
        }
        System.out.println("==============================================");
        //foreach(数据类型 变量名 ：使用Set()方法的变量名 )
        //建议使用这种方式，效率高，适合在大量数据环境下使用
        for (Map.Entry<Integer, String> node : it) {
            System.out.println(node.getKey() + "--->" + node.getValue());
        }
    }
}
```

