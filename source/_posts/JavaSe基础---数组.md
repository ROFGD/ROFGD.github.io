---
title: JavaSe基础---数组
date: 2020-05-01 18:41:01
author: ReadPond
tags:
- JavaSe
- 数组
categories:
- JavaSe
keywords: 
- 数组
- Java
top: false
index_img: /img/202301181833970.png
banner_img: /img/202301181833970.png
---

## 1 - 数组

### 1.1 - 数组的概念

	数组本质上就是在内存空间中申请一块连续的存储空间，用来记录多个类型相同的数据。

### 1.2 - 数组的创建

**定义方式**

```java
格式：  数据类型[] 数组名称 = new 数据类型[数组的长度];
int[] arr = new int[10];//定义了一个可以存储10个整数类型的数组
```

**初始化方式**

```java
格式：  数据类型[] 数组名称 = {成员1，成员2，成员3，成员4.....}；
int[] arr = {1,2,3,4,5,6,7,8,9,0};
```

**两种的结合方式**

```java
int[] arr = new int[]{0,9,8,7,6,5,4,3,2,1};
```

```java
import java.util.Arrays;

/**
 * @author ly_smith
 * @Description #TODO  数组的创建
 */
public class Demo04 {
    public static void main(String[] args) {
        int[] arr01 = new int[10];//定义了一个长度为10个数组来存储int类型的数据
        System.out.println(arr01);//引用中存储的是对象的地址
        //借助数组工具类中的toString方法打印数组中的内容
        System.out.println(Arrays.toString(arr01));

        //初始化方式创建数组
        int[] arr02 = {1,2,3,4,5,6,7,8,9,0};//会根据定义数组的成员的个数来申请相对应的存储空间
        System.out.println(Arrays.toString(arr02));

        //两种的结合方式
        int[] arr03 = new int[]{0,9,8,7,6,5,4,3,2,1};
        System.out.println(Arrays.toString(arr03));
    }
}
```

<a name="c14b7eb2"></a>

### 1.3 - 数组的访问与赋值

> 访问与赋值时通过**下标**来确定位置的，下标范围0 ~ 数组的长度减1结束


```java
import java.util.Arrays;

/**
 * @author ly_smith
 * @Description #TODO  数组的访问与赋值
 */
public class Demo05 {
    public static void main(String[] args) {
        //定义的方式创建数组
        int[] arr01 = new int[10];
        //数组中是存在默认值的
        System.out.println("数组中的默认值：" + Arrays.toString(arr01));
        //数组的赋值,在访问以及赋值时，[]中表示的是下标的概念
        arr01[5] = 6;
        arr01[8] = 9;

        System.out.println("数组赋值后的值：" + Arrays.toString(arr01));


        //使用初始化方式创建数组
        int[] arr02 = {1,2,3,4,5,6,7,8,9,0};
        //数组的访问
        System.out.println(arr02[3]);
        System.out.println(arr02[7]);

        //在数组的范围与赋值过程中，一定要小心使用下标，注意下标越界的问题
        //访问不存在的下标就会出现下标越界异常
//        System.out.println(arr02[11]);
    }
}
```

### 1.4 - 数组中的默认值

当数组定义完成后，数组会根据数据类型的不容而创建不同的默认值

| 数组中的数据类型      | 默认值        |
| --------------------- | ------------- |
| `byte short int long` | 0             |
| `float double`        | 0.0           |
| `boolean`             | false         |
| `char`                | '\\u0000'  空 |
| `String`              | null          |

### 1.5 - 数组的特性

	1、数组中的成员占用的是连续的存储空间
	
	2、数组名实为该数组的首地址
	
	3、数组的成员访问时，注意不要下标越界
	
	4、数组中的成员数据类型必须相同
	
	5、Java中的数组的长度一旦确定不能增减

<a name="6c066bf0"></a>

### 1.6 - 数组的遍历

```java
/**
 * @author ly_smith
 * @Description #TODO  数组的遍历
 */
public class Demo06 {
    public static void main(String[] args) {
        //初始化方式创建数组
        int[] score = {60,66,70,80,88,90,99,100};

        //通过for循环一次取出每个成员  -  正序
        for (int i = 0; i < score.length; i++) {  //score.length  获取数组的长度
            System.out.print("score[" + i + "] = " + score[i] + "\t");
        }

        System.out.println();
        System.out.println("----------------------------------------------------------");

        //数组逆序遍历
        for (int i = score.length - 1; i >= 0 ; i--) {
            System.out.print("score[" + i + "] = " + score[i] + "\t");
        }
    }
}
```

<a name="aa084921"></a>

### 1.7 - 数组的扩容以及拷贝

```java
import java.util.Arrays;

/**
 * @author ly_smith
 * @Description #TODO  数组的扩容以及拷贝
 */
public class Demo07 {
    public static void main(String[] args) {
        int[] arr01 = {1,2,3,4,5,6,7,8,9,0};//初始化方式创建数组

        //方法一
        int[] arr02 = new int[arr01.length * 2];//创建新数组，长度默认为原数组长度2倍
        //通过循环将原数组中的成员转移到新数组中
        for (int i = 0; i < arr01.length; i++) {
            arr02[i] = arr01[i];
        }

        System.out.println("arr01:" + Arrays.toString(arr01));//打印原数组
        System.out.println("arr02:" + Arrays.toString(arr02));//打印新数组

        System.out.println("----------------------------------------------------------------");

        //方法二:借助数组工具类Arrays中的方法
        //参数1：原数组的引用    参数2：新数组的长度   返回值：新数组的引用
        //兵器会帮助我们将原数组中的内容转移到新的数组中来
        int[] arr03 = Arrays.copyOf(arr01, arr01.length * 2);
        System.out.println("arr01:" + Arrays.toString(arr01));//打印原数组
        System.out.println("arr03:" + Arrays.toString(arr03));//打印新数组
    }
}
```

<a name="c2c3205f"></a>

### 1.8 - 数组中的经典算法

<a name="d019b4e0"></a>

#### 1.8.1 - 求最值

```java
import java.util.Arrays;
import java.util.Random;

/**
 * @author ly_smith
 * @Description #TODO  数组求最值
 */
public class Demo01 {
    public static void main(String[] args) {
        //通过随机类来生成数组中的数
        Random ran = new Random();   //随机数范围规定在0 ~ 99之间
        int[] arr = new int[10];//定义数组，长度为10

        //1、每循环一次生成一个随机数，存储到数组中相对应的下标位置
        for (int i = 0; i < arr.length; i++) {
            arr[i] = ran.nextInt(100);
        }

        //2、找到数组中的最大值
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            max = arr[i] > max ? arr[i] : max;//借助三目运算符
        }

        //打印数组
        System.out.println(Arrays.toString(arr));
        //3、最大值进行打印输出
        System.out.println("Max:" + max);
    }
}
```

<a name="8a064f1e"></a>

#### 1.8.2 - 查找数组中指定的成员的值（所在位置）

```java
import java.util.Arrays;
import java.util.Scanner;

/**
 * @author ly_smith
 * @Description #TODO  查找数组中指定的成员的值（所在位置）
 */
public class Demo02 {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5,6,7,8,9,6,0};//使用初始化方式创建数组

        System.out.println("数组中的成员有：" + Arrays.toString(arr));

        System.out.println("请输入要查找的值：");
        int num = new Scanner(System.in).nextInt();//通过文本扫描器来获取用户输入的整数类型数据

        int resIndex = 0;//用来记录值在数组中出现的位置
        //用num变量，与数组中的每个值做对比
        for (int i = 0; i < arr.length; i++) {
            if(num == arr[i]){//找值，如果找到了，则进入语句块中
                resIndex = i;
//                break;
                System.out.println(arr[resIndex] + "在数组中出现的位置是：[" + resIndex + "]");
            }
        }

//        System.out.println(arr[resIndex] + "在数组中第一次出现的位置是：[" + resIndex + "]");
    }
}
```

<a name="7634cf16"></a>

#### 1.8.3 - 数组的逆序存储

```java
/**
 * @author ly_smith
 * @Description #TODO  数组的逆序存储
 */
public class Demo03 {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5,6,7,8,9};//初始化方式创建数组

        System.out.print("逆序存储之前：");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + "\t");
        }

        //数组的逆序算法
        for (int i = 0,t; i < arr.length / 2; i++) {
            t = arr[i];
            arr[i] = arr[arr.length - 1 - i];
            arr[arr.length - 1 - i] = t;
        }

        System.out.println();
        System.out.print("逆序存储之后：");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + "\t");
        }
    }
}
```

<a name="b051b9ad"></a>

#### 1.8.4 - 数组的排序（冒泡排序法升序）

```java
import java.util.Arrays;
import java.util.Random;

/**
 * @author ly_smith
 * @Description #TODO  冒泡排序法（升序排序）
 */
public class Demo04 {
    public static void main(String[] args) {
        Random ran = new Random();//创建随机类对象
        int[] arr = new int[10];//定义数组，长度为10

        //使用随机类将数组中的成员填充满
        for (int i = 0; i < arr.length; i++) {
            arr[i] = ran.nextInt(100);//每循环一次，生成一个0 ~ 99之间的随机数，存放到数组中
        }

        System.out.println("数组中的成员有：" + Arrays.toString(arr));

        //排序算法
        for (int i = 0; i < arr.length - 1; i++) {//外循环循环一次，会找出一个最大值，控制循环次数的
            for (int j = 0; j < arr.length - 1 - i; j++) {//内循环控制前后连个数进行比较
                if(arr[j] > arr[j + 1]){//如果前面的值大于后面的值，将位置对调
                    int t = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = t;
                }
            }
        }

        System.out.println("冒 泡 排序 后：" + Arrays.toString(arr));
    }
}
```

<a name="83d0a242"></a>

#### 1.8.5 - 回文

```java
import java.util.Scanner;

/**
 * @author ly_smith
 * @Description #TODO  判断用户输入的是否是回文
 */
public class Demo05 {
    public static void main(String[] args) {
        System.out.println("请输入一串字符串："); //   a  b  c  d  d  c  b  a
        String str = new Scanner(System.in).nextLine();//获取用户输入的字符串

        //做回文的判断
        int i;
        for(i = 0; i < str.length() / 2; i++){
            if(str.charAt(i) != str.charAt(str.length() - 1 - i)){
                break;//如果不相等，则直接终止循环，
            }
        }

        //通过i记录的比较次数来判断是否是回文字符串
        if(i < (str.length() / 2)){
            System.out.println(str + "不是回文字符串");
        }else{
            System.out.println(str + "是回文字符串");
        }
    }
}
```

<a name="dcba9f40"></a>

#### 1.8.6 - 向有序数组中插入成员后仍保证数组有序

```java
int[] arr = {1,2,3,4,5,6,7,8,9,0};
```

```java
import java.util.Arrays;
import java.util.Scanner;

/**
 * @author ly_smith
 * @Description #TODO  向有序数组中插入成员后仍保证数组有序
 */
public class Demo01 {
    public static void main(String[] args) {
        //定义长度为10的数组
        int[] arr = new int[10];

        //使用for循环初始化数组
        for (int i = 0; i < arr.length - 1; i++) {
            arr[i] = i + 1;
        }

        System.out.println("插入前：" + Arrays.toString(arr));
        //获取用户想要插入的整数类型
        System.out.println("请输入要插入的数值：");
        int num = new Scanner(System.in).nextInt();

        //方法一：在末尾插入数据后，做冒泡排序法
//        arr[arr.length - 1] = num;
//        //冒泡排序法将数组排序
//        for (int i = 0; i < arr.length - 1; i++) {//控制比较的次数
//            for (int j = 0; j < arr.length - 1 - i; j++) {
//                if(arr[j] > arr[j + 1]){
//                    int t = arr[j];
//                    arr[j] = arr[j + 1];
//                    arr[j + 1] = t;
//                }
//            }
//        }

        //方法二：先找到成员要插入的位置 ，然后将该位置之后的成员一次向后移动，然后再将成员插入到指定的位置即可
        int place;
        for (place = 0;place < arr.length - 1;place++){
            if (num < arr[place]){
                break;
            }
        }//位置找到之后，退出循环，用place记录位置

        //将place位置之后的成员整体向后移动一位
        for (int i = arr.length - 2; i >= place ; i--) {
            arr[i + 1] = arr[i];
        }

        arr[place] = num;//将要插入的值放到指定下标的位置

        System.out.println("插入后：" + Arrays.toString(arr));
    }
}
```

<a name="18050b9a"></a>

## 2 - 二维数组

<a name="182fc4d4"></a>

### 2.1 - 二维数组的概念

	二维数组本质上就是由一维数组组成的数组，也就是说数组中的成员都是一个数组。

<a name="cb54b2ec"></a>

### 2.2 - 数组创建

**定义方式**

```java
int[][] arr = new int[2][3];//定义了一个2行3列的二维数组
*  *  *
*  *  *
```

**初始化方式**

```java
int[][] arr = {{1,2,3},{4,5,6}};//直接初始化2行3列的数组
1  2  3
4  5  6
```

```java
import java.util.Arrays;

/**
 * @author ly_smith
 * @Description #TODO  二维数组的创建
 */
public class Demo06 {
    public static void main(String[] args) {
        int[][] arr = new int[2][3];//定义的方式创建了一个2行3列的二维数组
        //使用遍历的方式查看二维数组中的内容
        for (int i = 0; i < 2; i++) {//外循环控制行
            for (int j = 0; j < 3; j++) {//内层循环控制列
                System.out.print(arr[i][j] + "\t");
            }
            System.out.println();
        }

        System.out.println("-------------------------------");

        //初始化方式创建二维数组
        int[][] arr02 = {{1,2,3},{4,5,6}};

        //使用双重嵌套循环来打印数组
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 3; j++) {
                System.out.print(arr02[i][j] + "\t");
            }
            System.out.println();
        }
    }
}
```

<a name="b5c17dd9"></a>

### 2.3 - 二维数组的赋值和访问

```java
/**
 * @author ly_smith
 * @Description #TODO  二维数组的访问与赋值
 */
public class Demo07 {
    public static void main(String[] args) {
        //定义的方式创建二维数组
        int[][] arr01 = new int[5][5];

        //循环赋值
        for (int i = 0; i < 5; i++) {
            for (int j = 0; j < 5; j++) {
                arr01[i][j] = j + 1;
            }
        }

        //循环遍历
        for (int i = 0; i < 5; i++) {
            for (int j = 0; j < 5; j++) {
                System.out.print(arr01[i][j] + "\t");
            }
            System.out.println();
        }

        //赋单个的值
        arr01[2][3] = 0;
        arr01[3][2] = 0;

        System.out.println("---------------------------");

        //循环遍历
        for (int i = 0; i < 5; i++) {
            for (int j = 0; j < 5; j++) {
                System.out.print(arr01[i][j] + "\t");
            }
            System.out.println();
        }

        //取值
        System.out.println(arr01[2][4]);

        //无论是数组还是字符串，下标或者位置的起始点都是0.
    }
}
```

<a name="4f9fb84e"></a>

### 2.4 - 二维数组 - 矩阵转置

```java
/**
 * @author ly_smith
 * @Description #TODO  二维数组 - 矩阵转置
 */
public class Demo08 {
    public static void main(String[] args) {
        //初始化方式创建数组
        int[][] arr = {{1,2,3},{4,5,6},{7,8,9}};

        //转置前的效果，遍历数组
        for (int i = 0; i < 3; i++) {//外循环控制行数
            for (int j = 0; j < 3; j++) {//内存换控制列数
                System.out.print(arr[i][j] + "\t");
            }
            System.out.println();
        }

        //转置效果
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if(j > i){//转置的条件
                    /*int t = arr[i][j];
                    arr[i][j] = arr[j][i];
                    arr[j][i] = t;*/

                    /*
                    位运算当中的方式，可以实现两个值的对调
                        a ^= b;
                        b ^= a;
                        a ^= b;

                    * */
                    arr[i][j] ^= arr[j][i];
                    arr[j][i] ^= arr[i][j];
                    arr[i][j] ^= arr[j][i];
                }
            }
        }

        System.out.println("-------------------");

        //转置后的效果，遍历数组
        for (int i = 0; i < 3; i++) {//外循环控制行数
            for (int j = 0; j < 3; j++) {//内存换控制列数
                System.out.print(arr[i][j] + "\t");
            }
            System.out.println();
        }
    }
}
```
