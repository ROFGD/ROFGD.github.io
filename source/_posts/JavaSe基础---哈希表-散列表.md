---
title: JavaSe基础---哈希表/散列表
date: 2020-05-16 15:30:48
author: ReadPond
tags:
- JavaSe
- Java哈希表
categories:
- JavaSe
keywords: 
- 哈希表
- Java
top: false
index_img: /img/202301191151469.png
banner_img: /img/202301191151469.png
---

### 哈希表/散列表 数据结构

-   1.HashMap集合底层是一个哈希表/散列表数据结构
-   2.哈希表是一个数组和单项链表得结合体。数组在查询方面效率很高，随机增删效率低，而单向链表在随机增删方面效率高，在查询方面效率低，哈希表则将以上两种优点结合，充分发挥各自优点

```java
public class HashMap{
	Node <k,v> [] table;
    static class Node <k,v>{
        final int hash; //哈希值
        final k key; //存储到Map集合中得Key
        V value; //存储到Map集合中得value
        Node<k,v> next; //下一个节点得内存地址
    }
}
```

-   4.哈希表底层是一个一维数组，这个数组中得每一个元素是一个单向链表(数组和链表得结合体)
-   5.**实现原理：**
    -   (a) map.put(k , v) 实现原理：
        -   先将k,v封装到Node对象当中，底层会调用k得hashCode()方法得出哈希值，然后通过哈希算法将哈希值转换成一个数组下标，下标位置上如果没有元素，就把Node添加到这个位置上，如果下标对应位置上有链表，此时会拿着K和链表上的每一个节点中的k进行equals，如果所有得equals方法返回都是false，那么这个新节点将被添加到末尾，如果返回都是true，那么这个节点的value将会被覆盖。
    -   (b) v = map.get(k)实现原理：
        -   先调用k的hashCode()方法得出哈希值，通过哈希算法转化成数组下标，通过数组下标快速定位到某个位置上，如果这个位置上什么也没有，返回null，如果这个位置上有单向链表，那么会拿着这个参数k和单向链表上的每一个节点中的k进行equals，如果返回的是false,那么get方法返回null，如果其中一个节点的k和参数k进行equals返回true，那么这个节点的value就是我们要找的value,get方法最终返回这个value
    -   **为什么哈希表增删效率以及查阅效率都很高？**
        -   因为增删在链表上完成，查询不需要都扫描，只需要部分扫描
-   6.通过上述可以得出HashMap集合中的key先调用两个方法，一个是HashCode()，一个是equals()，这两个方法都需要重写
-   7.HashMap集合的key部分特点：
    -   无序不可重复。无序是因为不一定挂到哪个链表上，不可重复是因为，equals方法来保证HashMap集合中的key不可重复，如果重复了value会覆盖，放在HashMap集合key部分元素就是放到HasSet中所以HashSet集合中元素也需要重写HashCode()+equals()方法中

-   8.哈希表HashMap使用不当时无法发挥性能!
    -   (1)假设将所有的HashCode()方法返回值固定为某个值，那么会导致底层哈希表变成了纯单向链表，这种情况我们称为散列分布不均匀。
        -   什么是散列均匀：
            -   假设有100个元素，10个单向链表那么每个单向链表上有10个节点，这是最佳情况
    -   (2)假设将所有的HashCode()方法返回值都设定为不一样的值，那么会导致底层变成纯一维数组，没有链表得概念，也是散列不均匀
    -   (3)散列分布均匀需要重写hasCode()方法时有一定得技巧。
    -   (4)**重点：放在****hasMp()****集合****key****部分的元素，以及放在****hashSet****集合中的元素，需要同时重写****hasCode****和****equals****方法。**
-   9.同一个单向链表上的节点的Hash值相同，因为他们的数组下标是一样的，但是同一个链表上的k和k的equals方法肯定返回false,都不相等。
    -    **放在****HashMap****集合的****k****部分元素，以及放在****HashSet****集合中的元素需要同时重写****hashCode()****和****equals()**
-   10.**HashMap****集合的默认初始化容量是****16****，默认加载因子是****0.75****，**这个默认加载因子是当数组容量达到75%时，开始扩容。**HashMap****集合初始化容量必须是****2****的倍数**，这也是官方推荐的，这是因为达到散列均匀，为了提高HashMap集合的存取效率
-   11.向Map集合中存，以及从Map集合中取，都先调用key的HashCode()方法，在调用equals方法
    -   用put(k,v)举例，什么时候不会调用equals方法？
        -   k.hashCode()方法返回哈希值，哈希值经过哈希算法转换成数组下标，数组下标位置上如果是null，则不需要执行(get(k)方法原理同上)
-   12.如果一个类的equals方法重写，那么HashCode(）方法也必须重写，并且equals方法返回true，hashCode()返回值也必须相同
    -   equals方法返回true，表示两个对象相同，在同一链表上比较，那么对于同一个单向链表上的节点来说，他们的哈希值都素hi相同的，所以hashCode()返回值也应该相同
-   13.对于HashMap在JDK8之后有新的改进：如果哈希表单向链表中的元素超过8个，单向链表这种数据结构就会变成红黑树数据结构，当红黑树上的节点少于6个时，会重新把红黑树变成单向链表。
-   14.hashMap()的key部分是否可以为空？
    -   可以，但是只允许有一个null，否则会覆盖
-   15.hashTable()的key和value是否可以为空？
    -   不能
-   16.**HashTable****的初始化容量是****11****，默认加载因子是****0.75f****，扩容是：原容量** *** 2 + 1**
-   17.**HashSet****集合初始化容量是****16****，初始化容量建议是****2****的倍数，扩容之后是原容量的****2****倍**

### Properties

-   1.Properties是一个Map集合，**继承**Hashtable，**Properties****的****key****和****value****都是****String****类型**
-   2.Properties被称为属性类对象
-   3.Properties是线程安全的
-   4.Properties也可以使用put()方法添加元素

```java
public class PropertiesText01 {
    public static void main(String[] args) {
        //创建对象
        Properties pro = new Properties();
        //存
        pro.setProperty("url","http://www.baidu.com");
        pro.setProperty("Name","Tom");
        pro.setProperty("Pass","78342ce");
        pro.setProperty("ID","0ox324234");

        //取
        String url = pro.getProperty("url");
        String Name = pro.getProperty("Name");
        String Pass = pro.getProperty("Pass");
        String ID = pro.getProperty("ID");

        System.out.println(url);
        System.out.println(Name);
        System.out.println(Pass);
        System.out.println(ID);

        System.out.println("==========================");

        System.out.println(pro.size());
    }
}

```

### TreeSet

-   1.TreeSet集合底层实际上是一个TreeMap
-   2.TreeMap集合底层实际上是一个二叉树
-   3.放到TreeSet集合中的元素，等同于放到TreeMap集合key部分了
-   4.TreeSet集合中的元素：无序不可重复，但是可以按照元素大小顺序自动排序----------可排序集合
-   5.**TreeSet****无法对自定义类型进行排序**，因为，没有自定义类型没有实现java.lang.Comparable接口，导致自定义类型在TreeMap集合中的put方法中向下转型的时候出现异常“java.lang.ClassCastException”

### 增强for循环和迭代器

>   for（数据类型：变量名 : 需要遍历的数组）{
>
>   ​			循环体;
>
>   }
>
>   **注意：在****IDE****中可以使用****iter + ent****键** **可以快速生成增强****for****循环**

-   1.什么是迭代器
    -   对过程进行重复，成为迭代.
    -   迭代器是遍历Collection集合的通用方式，可以对集合遍历的同时进行添加删除等操作
-   2.迭代器的常用方法
    -   next()：返回迭代的下一个元素对象
    -   hasNext():如果仍有元素可以迭代，则返回true
-   3.普通迭代器在遍历集合时不能进行添加或删除操作，否则会报：并发修改异常。列表迭代器遍历集合时可以进行添加或删除操作，但必须使用列表迭代器中的方法。如：

```java
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;
/*
* 需求：判断集合中是否包含“C”如果有则添加“java”
*
* */
public class LsitText01 {
    public static void main(String[] args) {
        //创建集合对象
        List list = new ArrayList();
        //添加元素
        list.add("A");
        list.add("B");
        list.add("C");
        list.add("F");
        //根据集合对象获取列表迭代器对象
        ListIterator iter = list.listIterator();
        //判断集合中是否有元素
        while (iter.hasNext()){
            //如果有就获取
			//为什么要向下转型：因为iter.next()是一个Object类型，但是我们添加的是一个字符串类型，所以需要向下转型
            String s = (String) iter.next();
            if ("C".equals(s)){
               //list.add("java"); ConcurrentModificationException-->并发修改异常
                iter.add("java");
            }
        }
        System.out.println(list);
    }
}

```

