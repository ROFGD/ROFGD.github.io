---
title: JavaSe基础---IO流
date: 2020-05-11 18:45:41
author: ReadPond
tags:
- JavaSe
- IO流
categories:
- JavaSe
keywords: 
- IO流
- Java
top: false
index_img: /img/202301181833972.png
banner_img: /img/202301181833972.png
---

## 1 - File类

### 1.1 - 基本概念

	java.io.File类用于描述文件和目录的路径信息，可以获取文件大小等相关属性。

### 1.2 - 构造方法

| 方法名                             | 说明                                             |
| ---------------------------------- | ------------------------------------------------ |
| `File(String pathname)`            | 通过给定的路径名转换为File对象                   |
| `File(String parent,String child)` | 从父类的路径名和子类的文件名来创建一个File对象   |
| `File(File parent,String child)`   | 从父类的Flie目录和子类的文件名来创建一个File对象 |


```java
import java.io.File;

/**
 * @author ly_smith
 * @Description #TODO  File对象的构造器
 */
public class Demo02 {
    public static void main(String[] args) {
//        相对路径与绝对路径
        /*
        相对路径：相对于当前目录的某一级目录
        当前路径表示为：  ./   (Idea)默认从项目根目录开始查找
        上一级目录表示为：../
        上一级的上一级目录表示为：../../

        绝对路径：从磁盘根目录开始查找
        E:\\workingspace\\bjpn2209\\com.bjpowernode.day06.file\\src\\Demo02.java
        E:/workingspace/bjpn2209/com.bjpowernode.day06.file/src/Demo02.java
        * */

        //与文件向关联，并不会创建或获取文件
        File file = new File("./myTest.txt");
        System.out.println(file);

        //第一个参数是路径，第二个参数是要操作的目标文件名
        File file1 = new File("./", "myTest.txt");
        System.out.println(file1);

        File file2 = new File("./");//将路径构造成File对象
        File file3 = new File(file2, "myTest.txt");
        System.out.println(file3);
    }
}
```

<a name="c27a3d0c"></a>

### 1.3 - File类的创建功能

| 方法名                    | 说明             |
| ------------------------- | ---------------- |
| `boolean createNewFile()` | 创建文件         |
| `boolean mkdir()`         | 创建目录（单个） |
| `boolean mkdirs()`        | 创建目录（多级） |


```java
import java.io.File;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO File类的创建功能
 */
public class Demo03 {
    public static void main(String[] args) throws IOException {
        //创建文件前需要与文件相关联
        File file = new File("./myTest.txt");
        if (file.createNewFile()){
            System.out.println("文件创建成功！");
        }else{
            System.out.println("文件创建失败！");
        }

        //创建单个目录，先构造File对象，与目录相关联
        File myDir = new File("./", "myDir");
        //创建多级目录的方式可以创建单个目录
        if (myDir.mkdirs()){
            System.out.println("目录创建成功！");
        }else{
            System.out.println("目录创建失败！");
        }

        File file1 = new File("./H/E/L/L/O");
        //创建单个目录的方式不能创建多级目录的
//        if (file1.mkdir()){
        if (file1.mkdirs()){
            System.out.println("多级目录创建成功~");
        }else{
            System.out.println("多级目录创建失败~");
        }
    }
}
```

<a name="48e68535"></a>

### 1.4 - File类的常用功能

| 方法名                     | 说明                                                     |
| -------------------------- | -------------------------------------------------------- |
| `boolean isDirectory()`    | 判断一个File对象指向的是否是一个目录                     |
| `boolean isFile()`         | 判断一个File对象指向的是否是一个文件                     |
| `boolean exists()`         | 判断一个File对象指向的文件或目录是否存在，存在则返回true |
| `String getAbsolutePath()` | 获取File对象指向的文件或目录的绝对路径                   |
| `String getPath()`         | 获取创建File对象时候使用的路径和文件名                   |
| `String getName()`         | 获取创建FIle对象的文件名                                 |
| `String getParent（）`     | 获取创建File对象所在的目录名                             |
| `File[] listFiles()`       | 获取File对象中所有的文件（File对象形式）                 |
| `long lastModified()`      | 获取到File对象中文件最后被修改的时间                     |


```java
import java.io.File;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * @author ly_smith
 * @Description #TODO  File类中的常用功能
 */
public class Demo04 {
    public static void main(String[] args) {
        //先构造一些File对象与目录或文件向关联
        File file = new File("./");
        File file1 = new File("./a.txt");
        File file2 = new File("./myTest.txt");
        File file3 = new File("./myDir");

        //判断是否是目录
        System.out.println(file.isDirectory());//true
        System.out.println(file1.isDirectory());
        System.out.println(file2.isDirectory());
        System.out.println(file3.isDirectory());//true
        System.out.println("----------------------------------------------");
        //判断是否是文件
        System.out.println(file.isFile());//false
        System.out.println(file1.isFile());//false,因为文件不存在，所以并不能确定是文件
        System.out.println(file2.isFile());//true
        System.out.println(file3.isFile());//false
        System.out.println("----------------------------------------------");
        //判断是否存在
        System.out.println(file.exists());
        System.out.println(file1.exists());//false
        System.out.println(file2.exists());
        System.out.println(file3.exists());
        System.out.println("----------------------------------------------");
        //获取绝对路径
        System.out.println(file.getAbsolutePath());
        System.out.println(file1.getAbsolutePath());
        System.out.println(file2.getAbsolutePath());
        System.out.println(file3.getAbsolutePath());
        System.out.println("----------------------------------------------");
        //获取相对路径，会根据构造FIle对象时参数决定的
        System.out.println(file.getPath());
        System.out.println(file1.getPath());
        System.out.println(file2.getPath());
        System.out.println(file3.getPath());
        System.out.println("----------------------------------------------");
        //获取文件后目录名
        System.out.println(file.getName());
        System.out.println(file1.getName());
        System.out.println(file2.getName());
        System.out.println(file3.getName());
        System.out.println("----------------------------------------------");
        //获取当前文件所在的目录
        System.out.println(file.getParent());
        System.out.println(file1.getParent());
        System.out.println(file2.getParent());
        System.out.println(file3.getParent());
        System.out.println("----------------------------------------------");
        //listFiles，查看目录中的所有文件
        File[] files = file.listFiles();
        for (int i = 0; i < files.length; i++) {
            System.out.println(files[i]);
        }

        System.out.println("----------------------------------------------");
        //获取文件最后被修改的时间
        Date date = new Date(file2.lastModified());//将long的毫秒数构造成Date对象
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//构造格式化输出的对象
        String format = sdf.format(date);//将date对象按照调用对象的格式进行格式化输出，并以字符串形式返回
        System.out.println(format);
    }
}
```

<a name="3ce108d4"></a>

### 1.5 - File类的删除功能

| 方法名             | 说明                 |
| ------------------ | -------------------- |
| `boolean delete()` | 删除文件或（空）目录 |


```java
import java.io.File;

/**
 * @author ly_smith
 * @Description #TODO  File类的删除功能
 */
public class Demo05 {
    public static void main(String[] args) {
//        File file = new File("./a.txt");//构造一个不存在的文件对象
//        File file = new File("./myTest.txt");//构造一个存在的文件
//        File file = new File("./myDir");//构造一个存在的空目录
        File file = new File("./H");//构造了一个多级目录的

        //删除文件之前需要先判断文件是否存在
        if(file.exists()){
            if (file.delete()){
                System.out.println(file.getName() + "删除成功！");
            }else{
                System.out.println(file.getName() + "删除失败！");
            }
        }else{
            System.out.println(file.getName() + "不存在！");
        }
    }
}
```

<a name="a8793ac6"></a>

### 1.6 - 目录的递归删除

```java
import java.io.File;

/**
 * @author ly_smith
 * @Description #TODO  目录的递归删除
 */
public class Demo06 {
    public static void main(String[] args) {
        final String SRC = "./H";//被删除的目录存储成常量
        File file = new File(SRC);//构造File对象，与H目录相关联
        delDir(file);//调用方法删除H多级目录
    }

    /**
     * 递归删除目录
     * @param file 要删除的File对象
     */
    private static void delDir(File file) {//H
        //获取当前目录中的文件列表
        File[] files = file.listFiles();
        //判断数组的长度，可以直到目录中是否有内容，如果没有则直接删除
        if(files.length > 0){//有内容，则遍历内部文件对象
            for (int i = 0; i < files.length; i++) {
                if(files[i].isDirectory()){//遍历文件中的对象，并判断是否是目录
                    delDir(files[i]);//如果是目录，需要在调用递归，打开并查看目录
                }else{
                    files[i].delete();//如果是文件可以直接删除文件
                }
            }
        }
        file.delete();//如果没有内容则之间删除目录即可
    }
}
```

<a name="340f0d5f"></a>

### 1.7 - 修改文件后缀名

- 将指定目录下的所有文件后缀名为txt统一修改成md为结尾的文件。

```java
import java.io.File;

/**
 * @author ly_smith
 * @Description #TODO 将指定目录下的所有文件后缀名为txt统一修改成md为结尾的文件
 */
public class Demo07 {
    public static void main(String[] args) {
        File file = new File("./dir");//构造file对象与dir目录相关联
        renameFile(file);
    }

    private static void renameFile(File file) {
        if(file.exists()){//判断file对象指向的目录是否存在
            //取出dir目录中的所有内容，一file对象形式返回
            File[] files = file.listFiles();
            for (File f : files) {//遍历数组中的文件
                if(f.isFile()){//判断是否是文件
                    if(f.getName().endsWith("txt")){//判断文件名是否以txt为结尾
                        String parent = f.getParent();//当前文件所在的目录名
                        String name = f.getName();//获取当前文件名

                        //修改文件后缀名  name  abc.txt
                        String newName = name.substring(0, name.lastIndexOf(".")) + ".md";
                        File file1 = new File(parent, newName);//将新的文件名构造成file对象
                        f.renameTo(file1);//使用renameTo方式实现文件重命名
                    }
                }else{
                    renameFile(f);//如果是目录就递归调用自己
                }
            }
        }
    }
}
```

**思考题**

递归实现查看指定目录中的所有文件以及目录的绝对路径

<a name="bd216a7c"></a>

## 2 - I/O字节流

<a name="f81c8f28"></a>

### 2.1 - I/O分类

> I/O : Input(输入)  /  Output(输出)

> 流：是一种抽象的概念，表示数据传输的总称，就是说数据在设备之间的传输称之为流，流的本质就是传输


- 根据数据的流向

    - 输入流：读数据
    - 输出流：写数据

> 站在内存的角度，以内存为基准去理解


- 根据数据类型

    - 字节流（万能流）：所有通过文本编译器打开之后看不懂的都是字节流

        - 字节输入流
        - 字节输出流
    - 字符流：通过文本编译器打开之后能看懂的都是字符流

        - 字符输入流
        - 字符输出流

<a name="b1091661"></a>

### 2.2 - 字节流写数据

- `OutputStream` - 字节输出流

| 方法名                                         | 说明                                           |
| ---------------------------------------------- | ---------------------------------------------- |
| `FileOutputStream(String name)`                | 根据参数指定的路径来构造对象并关联起来         |
| `FileOutputStream(String name,boolean append)` | 以追加的方式构造对象                           |
| `void write(int b)`                            | 用于将参数指定的单个字节写入到输出流中         |
| `void write(byte[] bytes)`                     | 用于将参数指定的字节数组内容全部写入到输出流中 |
| `void write(byte[] bytes, int off , int len)`  | 用于将数组中的一部分内容写入到输出流中         |
| `void close()`                                 | 关闭流并释放资源                               |


```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  字节流写数据
 */
public class Demo02 {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("./myTest.txt");//构造字节输出流对象,覆盖写
//        FileOutputStream fos = new FileOutputStream("./myTest.txt",true);//构造字节输出流对象,追加写

        //一次写一个字节，通过字符编码写入文件
//        fos.write(65);
//        fos.write(66);
        //将26个英文字母写入到文件中
//        for (int i = 0; i < 26; i++) {
//            fos.write(65 + i);
//        }

        //写整个byte数组
//        byte[] bytes = {97,98,99,100,101,102,103};
//        fos.write(bytes);

        //写byte数组中的一部分内容
        byte[] bytes = "123456789腾讯".getBytes();
        //参数二：表示起始位置，参数三：表示写多长的数据
        fos.write(bytes,5,7);
        fos.close();//关闭流

        //在不同的编码格式下，中文占用的字节数是不同的
        //在UTF-8编码格式下，一个中文使用3个字节来表示
        //在GBK编码格式下，一个中文使用2个字节来表示
    }
}
```

<a name="a0e88b4a"></a>

### 2.3 - 字节流异常处理

- 抛出去  或者  try、catch
- 通过finally 来 close

```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO   字节流异常处理
 */
public class Demo03 {
    public static void main(String[] args) {
        FileOutputStream fos = null;//构造字节输出流对象,覆盖写
        try {
            fos = new FileOutputStream("./myTest.txt");
            //写byte数组中的一部分内容
            byte[] bytes = "123456789腾讯".getBytes();
            //参数二：表示起始位置，参数三：表示写多长的数据
            fos.write(bytes,5,7);
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                fos.close();//关闭流
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

<a name="66bf7403"></a>

### 2.4 - 字节流读数据

- `InputStream` - 字节输入流

| 方法名                                      | 说明                                       |
| ------------------------------------------- | ------------------------------------------ |
| `FileInputStream(File file)`                | 根据参数指定的File对象来构造字节输入流对象 |
| `FileInputStream(String name)`              | 根据参数指定的字符串来构造字节输入流对象   |
| `int read()`                                | 用于从输入流中读取单个的字节数据           |
| `int read(byte[] bytes)`                    | 用于从输入流中读取数组长度个字节数组       |
| `int read(byte[] bytes , int off ,int len)` | 用于从输入流中读取数组中一部分内容         |
| `void close()`                              | 关闭流并释放资源                           |


```java
import java.io.FileInputStream;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  字输入流读数据
 */
public class Demo04 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("./myTest.txt");//创建字节输入流对象

        //一次读取一个字节，返回的是字符编码
//        int by = fis.read();
//        //如果读到了-1 ，说明文件中有效的内容已经读完了。
//        System.out.println((char) by);

        //循环的方式读取单个字节
//        int by;//用来存储字符编码
//        while ((by = fis.read()) != -1){//如果没有数据可读了，则会返回-1
//            System.out.println((char)by);
//        }

        //一次读取一个数组
        byte[] bytes = new byte[1024];//创建一个数组来存储读取到的数据
        int len;//用来存储读取到的数据的实际长度
//        len = fis.read(bytes);
//        //将byte数组转换成字符串并打印输出
//        System.out.println(new String(bytes,0,len));

        //循环的方式一次读取一个数组
        while ((len = fis.read(bytes)) != -1){
            System.out.println(new String(bytes,0,len));
        }

        fis.close();//关闭流并释放资源
    }
}
```

- 字节流一次读写一个字节

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  字节流一次读写一个字节
 */
public class Demo05 {
    public static void main(String[] args) throws IOException {
        //创建字节输入/输出流对象
        FileInputStream fis = new FileInputStream("./myTest.txt");
        FileOutputStream fos = new FileOutputStream("./newTest.txt");

        //读写操作
        int by;//存储字符编码
        while ((by = fis.read()) != -1){//第一个字节
            fos.write(by);//通过输出流写到关联的文件中
        }

        //关闭流
        fis.close();
        fos.close();
    }
}
```

- 复制图片

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author ly_smith
 * @Description #TODO  复制图片
 */
public class Demo06 {
    public static void main(String[] args) throws IOException {
        //创建字节输入输出流对象
        FileInputStream fis = new FileInputStream("./pic.png");
        FileOutputStream fos = new FileOutputStream("./newPic.png");

        //使用单个字节来赋值图片
//        int by;
//        while ((by = fis.read()) != -1){//读一个字节
//            fos.write(by);//写一个字节
//        }

        //使用一次读写一个数组
        byte[] bytes = new byte[1024];//计算机中的单位进制位是1024
        int len;//用来存储数组中读取到的实际数据的长度
        while ((len = fis.read(bytes)) != -1){//读一个数组
            fos.write(bytes,0,len);//写一个数组
        }

        //关闭流
        fis.close();
        fos.close();
    }
}
```

<a name="5dbe036b"></a>

## 3 - I/O字符流

<a name="3c1b07bb"></a>

### 3.1 - 字符输出流写数据

- OutputStreamWriter`

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

/**
 * @author ly_smith
 * @Description #TODO  字符流写数据
 */
public class Demo01 {
    public static void main(String[] args) throws IOException {
        //构造字符输出流对象
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("./myTest.txt"));//覆盖写
//        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("./myTest.txt",true));//追加写

        //写一个字符
//        osw.write(65);
//        osw.write('Z');

        //写一个字符串
//        osw.write("你好，JavaSE！");
//        osw.write("1234567890",2,6);//写字符串中一部分内容，参数二：起始位置  参数三：写数据的长度

        //写一个字符数组
        char[] chs = {'a','b','c','d','e','f','g'};
//        osw.write(chs);//写一整个char数组
        osw.write(chs,4,2);

        osw.close();//关闭流
    }
}
```

<a name="d994138b"></a>

### 3.2 - 字符输入流读数据

- `InputStreamReader`

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * @author ly_smith
 * @Description #TODO  字符输入流读数据
 */
public class Demo02 {
    public static void main(String[] args) throws IOException {
        //构造字符输入流对象
        InputStreamReader isr = new InputStreamReader(new FileInputStream("./myTest.txt"));

        //使用循环，一次读取一个字符
//        int ch;//用来存储字符编码
//        while ((ch = isr.read()) != -1){
//            System.out.print((char) ch);
//        }

        //一次读取一个数组
        char[] chs = new char[1024];
        int len;
        while ((len = isr.read(chs)) != -1){
            System.out.println(new String(chs,0,len));
        }

        isr.close();//关闭流
    }
}
```

<a name="3af12da4"></a>

### 3.3 - 字符流的文本拷贝

```java
import java.io.*;

/**
 * @author ly_smith
 * @Description #TODO  字符流的文本拷贝
 */
public class Demo03 {
    public static void main(String[] args) throws IOException {
        //构造字符输入输出流对象
        InputStreamReader isr = new InputStreamReader(new FileInputStream("./myTest.txt"));
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("./newTest.txt"));

        //一次读写单个字符
//        int ch;//存储字符编码
//        while ((ch = isr.read()) != -1) {//读一个字符
//            osw.write(ch);//写一个字符
//        }

        //一次读写单个字符数组
        char[] chs = new char[1024];
        int len;//存储读取到的数据的长度
        while ((len = isr.read(chs)) != -1){//读一个数组
            osw.write(chs,0,len);//写个一个数组
        }

        //关闭流
        isr.close();
        osw.close();
    }
}
```

<a name="b0a6314f"></a>

### 3.4 - 字符流的简化写法

`FileReader FileWriter`

```java
import java.io.*;

/**
 * @author ly_smith
 * @Description #TODO  字符流的简化写法
 */
public class Demo04 {
    public static void main(String[] args) throws IOException {
        //字符流的正常写法
//        InputStreamReader isr = new InputStreamReader(new FileInputStream("./a.txt"));
//        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("./a.txt"));
        //通过简化方式构造字符输出输入流对象
        FileReader fr = new FileReader("./myTest.txt");
        FileWriter fw = new FileWriter("./newTest.txt");

        //一次读写一个字符
//        int ch;
//        while ((ch = fr.read()) != -1){//读一个字符
//            fw.write(ch);//写一个字符
//        }

        //一次读写一个数组
        char[] chs = new char[1024];
        int len;
        while ((len = fr.read(chs)) != -1){//读一个数组
            fw.write(chs,0,len);//写一个数组
        }

        //关闭流
        fr.close();
        fw.close();
    }
}
```

<a name="72486787"></a>

### 3.5 - 缓冲区高效读写

| 方法名                          | 说明                             |
| ------------------------------- | -------------------------------- |
| `BufferedReader(Reader reader)` | 根据参数构造缓冲区字符输入流对象 |
| `BufferedWriter(Writer writer)` | 根据参数构造缓冲区字符输出流对象 |


- 缓冲区字符流特有方法

     - `BufferedWriter`  - `void newLine()` 插入换行符
        <br />				- `void flush()`  刷新输出流

     - `BufferedReader` - `String readLine()`读取一行


```java
import java.io.*;

/**
 * @author ly_smith
 * @Description #TODO  缓冲区高效读写
 */
public class Demo05 {
    public static void main(String[] args) throws IOException {
        //构造缓冲区字符输入流对象
//        BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream("./myTest.txt")));
        //构造缓冲区字符输出流对象
//        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("./newTest.txt")));

        //简单方法
        BufferedReader br = new BufferedReader(new FileReader("./myTest.txt"));
        BufferedWriter bw = new BufferedWriter(new FileWriter("./newTest.txt"));

        String line;//用来存储读取出来的一行数据的
        while ((line = br.readLine()) != null){//如果读不到数据说明读到了文件的末尾，并且返回null
            bw.write(line);//写一行
            bw.newLine();//换一行
            bw.flush();//刷新流
        }

        //关闭流
        br.close();
//        bw.close();//释放资源的同时，也会有刷新流的动作
    }
}
```

<a name="73fe3001"></a>

## 4 - 标准输入/输出/错误流

- System.in

```java
import java.io.*;
import java.util.Scanner;

/**
 * @author ly_smith
 * @Description #TODO  标准输入流  System.in
 */
public class Demo06 {
    public static void main(String[] args) throws IOException {
//        String line = new Scanner(System.in).nextLine();
//        System.out.println(line);

//        InputStream in = System.in;//字节流
//        InputStreamReader isr = new InputStreamReader(in);//字符流
//        BufferedReader br = new BufferedReader(isr);//将普通字符流包装成缓冲区字符输入流

        //自己将标准输入流封装成缓冲区字符输入流
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        //创建缓冲区字符输出流对象与文件向关联
        BufferedWriter bw = new BufferedWriter(new FileWriter("./myTest.txt"));

        String str;
        while (true){
            if((str = br.readLine()).equals("over")){
                break;//退出循环
            }

            bw.write(str);//写一行
            bw.newLine();//插入换行符
            bw.flush();//刷新流
        }

        //关闭流
        br.close();
        bw.close();
    }
}
```

- System.out

```java
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.PrintStream;

/**
 * @author ly_smith
 * @Description #TODO  标准输出流  System.out
 */
public class Demo07 {
    public static void main(String[] args) throws IOException {
        System.out.println("正常打印");

        PrintStream out = System.out;//字节流
        //字节通过缓冲区字符流封装的标准输出流
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        bw.write("Hello JavaSE!");

        bw.close();//关闭流，刷新流
    }
}
```

- System.err

```java
/**
 * @author ly_smith
 * @Description #TODO  标准错误流  System.err
 */
public class Demo08 {
    public static void main(String[] args) {
        System.out.println("标准输出流");
        System.err.println("标准错误流");//输出优先级高于普通输出流，但并不绝对，

        //尝试着自己封装
    }
}
```

<a name="218fbe9b"></a>

## 5 - 对象的持久化存储

<a name="41cb1b73"></a>

### 5.1 - 通过之前学过的知识去做对象的持久化存储

```java
import java.io.*;

/**
 * @author ly_smith
 * @Description #TODO  通过之前学过的知识完成对象的持久化存储的工作
 */
public class Demo01 {
    public static void main(String[] args) throws IOException {
//        mySave();//存对象中的内容

        Student newStu = myLoad();//加载对象
        System.out.println(newStu);
    }

    /**
     * 将文件中的对象信息加载出来
     * @return
     */
    private static Student myLoad() throws IOException {
        //构造缓冲区字符输入流对象
        BufferedReader br = new BufferedReader(new FileReader("./myTest.txt"));
        String str = br.readLine();//  Andy##18
        //拆分字符串
        String[] sp = str.split("##"); // sp[0]:姓名  sp[1]:年龄

        br.close();//关闭流并释放资源

        //将学生信息组成新的学生对象并进行返回
        return new Student(sp[0],Integer.parseInt(sp[1]));
    }

    /**
     * 将一个自定义的对象存储到文件中
     */
    private static void mySave() throws IOException {
        Student stu = new Student("Andy", 18);//构造学生对象
        //自己封装一个缓冲区字符输出流对象
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("./myTest.txt")));

        //将学生姓名和年龄分别取出后写入到文件中
        bw.write(stu.getName() + "##" + stu.getAge());

        bw.close();//关闭流并释放资源
    }
}
```

<a name="e6f533f2"></a>

### 5.2 -  对象的序列化操作

| 方法名                                 | 说明                           |
| -------------------------------------- | ------------------------------ |
| `ObjectOutputStream(OutputStream out)` | 通过参数构造序列化对象（写）   |
| `ObjectInputStream(InputStream in)`    | 通过参数构造反序列化对象（读） |


```java
import java.io.*;

/**
 * @author ly_smith
 * @Description #TODO  使用对象的序列化工具进行对象的存取
 */
public class Demo02 {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
//        Student stu = new Student("Bob", 21);//构造学生对象
//        //使用序列化（串行化）将学生对象写入到文件中
//        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("./myTest.txt"));
//
//        //写学生对象
//        oos.writeObject(stu);
//
//        oos.close();//关闭流

        System.out.println("-------------------------------------");

        //使用反序列化（反串行化）将文件中的学生对象加载到内存中来
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("./myTest.txt"));

        Student newStu = (Student) ois.readObject();

        System.out.println(newStu);

        ois.close();//关闭流并释放资源
    }
}
```
