---
title: JavaSe基础---IO流
date: 2020-07-11 18:45:41
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

### IO流

#### 1.分类

-   （1）按照流的方向进行分类：以内存为参照物，往内存中去的叫输入(读)，从内存中出来的叫做输出(取)
-   （2）按读取的数据方式的不同进行分类：有的流是按字节的方式进行读取，一次读取1个字节(byte)，等同于一次读取8个2进制位，这种流是万能的，什么类型文件都能读取；有的流是按字符方式读取数据，一次读取1个字符，这种流是为了方便读取文本文件而存在，但不能读取图片，音频等，**word****文件也无法读取**
-   综上所述，流的分类有：输入输出流，字符字节流

#### 2.流的四大家族

-   Java.io.InputStream---------->字节输入流
-   Java.io.OutputStream---------->字节输出流
-   Java.io.Reader---------->字符输入流
-   Java.io.Writer---------->字符输出流
-   **注意：所有的流都有****close()****方法，用完流一定要关闭！！！**

#### 3.所有的输出流都实现了：

-   **Java.io.Flushable**接口，都是可刷新的，都有**flush()**方法，输出流在最终输出之后，**一定要写****flush()****方法****,****表示将管道当中的输出数据强行输出完**。这个方法**作用是清空管道**

#### 4.常用流

-   (1)文件流
    -   Java.io.FileInputStream
    -   Java.io.OutputStream
    -   Java.io.FileReader
    -   Java.io.FileWriter
-   (2)转换流(字节流---->字符流)
    -   Java.io.InputStreamReader
    -   Java.io.OutputStreamWriter
-   (3)缓冲流
    -   Java.io.BufferedReader
    -   Java.io.BufferedWriter
    -   Java.io.BufferedInputStream
    -   Java.io.BufferedOutputStream
-   (4)数据流
    -   Java.io.DataInputStream
    -   Java.io.DataOutputStream
-   (5)标准输入输出流
    -   Java.io.PrintWriter
    -   Java.io.PrintStream
-   (6)对象流
    -   Java.io.ObjectInputStream
    -   Java.io.ObjectOutputStream

```java
public class FileInputStreamText01 {
    public static void main(String[] args) {
        FileInputStream fis = null;
        try {
             fis = new FileInputStream("D:\\Text\\联系.txt");

             //读取流
            int FileData = fis.read();
            System.out.println(FileData);
        } catch (FileNotFoundException e) {
             e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fis != null) {
                //在finally语句块儿中确保流一定关闭
                //关闭条件：流不为空,如果流为空就不用关闭
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/*
* 1.文件字节输出流
* 2.内存---->硬盘
* 3.void	close()------关闭此文件输出流并释放与此流相关联的任何系统资源。
* 4.protected void	finalize()------清理与文件的连接，并确保当没有更多的引用此流时，将调用此文件输出流的 close方法。
* 5.FileChannel	getChannel()------返回与此文件输出流相关联的唯一的FileChannel对象。
* 6.FileDescriptor	getFD()------返回与此流相关联的文件描述符。
* 7.void	write(byte[] b)------将 b.length个字节从指定的字节数组写入此文件输出流。
* 8.void	write(byte[] b, int off, int len)------将 len字节从位于偏移量 off的指定字节数组写入此文件输出流。
* 9.void	write(int b)------将指定的字节写入此文件输出流。
* */
public class FileOutputStreamText01 {
    public static void main(String[] args) {
        FileOutputStream fos = null;
        try {
            //加入true表示不更新源文件直接在源文件后面加入需要写入内容，不加true，表示需要先将源文件清空然后在重新写入新的字符
            fos = new FileOutputStream("D:\\Text\\联系.txt",true);
            //开始写文件
            String s = "我爱你，但是我现在需要变得更好！！！";
            //将字符串转换成byte数组
            byte[] byt = s.getBytes();
            fos.write(byt);
            //输出流必须加入刷新
            fos.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

public class BufferReaderText02 {
    public static void main(String[] args) {
        //文件字节输入流 ----> 使用缓冲流(BufferReader) ----> 转换为文件字符输入流
        FileInputStream fileInputStream = null;
        try {
          /*  //这是个字节流
            fileInputStream = new FileInputStream("D:\\Text\\联系.txt");
            //这是转换流
            InputStreamReader inputStreamReader = new InputStreamReader(fileInputStream);
            //这是个字符流
            BufferedReader bufferedReader = new BufferedReader(inputStreamReader);*/

            //合并写法
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(new FileInputStream("D:\\\\Text\\\\联系.txt")));

            String s1 = null;
            while ((s1 = bufferedReader.readLine()) != null){
                System.out.println(s1);
            }

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fileInputStream != null) {
                try {
                    //关闭流
                    fileInputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

public class BufferWriterText01 {
    public static void main(String[] args) {
        //文件字节输出流 ----> 使用缓冲流(BufferReader) ----> 转换为文件字符输出流
        FileOutputStream fileOutputStream = null;
        try {
            fileOutputStream = new FileOutputStream("D:\\Text\\java01.text",true);
            OutputStreamWriter outputStream = new OutputStreamWriter(fileOutputStream);
            BufferedWriter bufferedWriter = new BufferedWriter(outputStream);
            bufferedWriter.write("HEllo,world\n");
            bufferedWriter.write("Number N0.1\n");

            bufferedWriter.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (fileOutputStream != null) {
                try {
                    fileOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

//单个文件拷贝
public class IoCopyText01 {
    public static void main(String[] args) {
        FileOutputStream fio = null;
        FileInputStream fis = null;
        try {
            //创建一个输入流对象
            fis = new FileInputStream("C:\\Users\\11026\\Desktop\\1611539565799.docx");
            //创建一个输出流对象
            fio = new FileOutputStream("D:\\Text\\1611539565799.docx");

            //一边读一边写
            byte[] byt = new byte[1024 * 1024]; //一次最多拷贝1MB
            int count = 0;
            while((count = fis.read(byt)) != -1){
                fio.write(byt,0,count);
            }

            //刷新，输出流最后要刷新
            fio.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            //这里需要分开关闭，否则会影响另一个流的关闭
            if (fio != null) {
                try {
                    fio.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

public class CopyText02 {
    public static void main(String[] args) {
        FileWriter in = null;
        FileReader out = null;
        //创建读，写对象
        try {
            in = new FileWriter("D:\\Text\\NEWS.txt");
            out = new FileReader("D:\\NEWS.txt");
            //边读边写
            char[] chars = new char[4];
            int readCount = 0;
            while((readCount = out.read(chars)) != -1){
                in.write(chars,0,readCount);
            }
            //刷新
            in.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (out != null) {
                try {
                    out.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (in != null) {
                try {
                    in.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}


```

#### 5.File类常用的方法

-   1.exists()----->判断文件是否存在
-   2.createNewFile()-------->以文件的形式创建
-   3.mkdir()------->以目录形式创建
-   4.mkdirs()-------->创建多重目录
-   5.getParent()------->获取文件的父路径
-   6.getParentFile()------>获取文件的绝对路径
-   7.getAbsolutePath()------->获取计算机任意一个文件的绝对路径
-   8.getName()----->获取文件名
-   9.isFile()----->判断是否是一个文件
-   10.isDirectory()------->判断是否是一个目录
-   11.lastModified()------->获取文件的最后修改时间
-   12.length()------>获取文件大小(字节)
-   13.listFiles()----------->获取当前路径下的子文件

```java
public class FileText01 {
    public static void main(String[] args) {
        //创建file对象
        File file = new File("D:\\Text\\java_File");
        //判断D:\java_File是否存在
        System.out.println(file.exists()); //false

        //如果不存在，则以文件形式创建
       /* if(!file.exists()){
            try {
                file.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }*/
        //如果不存在，则以目录形式创建
        if(!file.exists()){
            file.mkdir();
        }
        //创建多重目录
        /*File file1 = new File("D:\\Text\\a\\b\\c\\d\\e\\f\\g\\h\\i");
        if (!file1.exists()){
            file1.mkdirs();
        }*/

        File file1 = new File("D:\\JAVAText\\src\\列车牵引计算\\计算牵引质量\\Mass.java");
        //获取文件的父路径
        System.out.println("相对路径：" + file1.getParent());//D:\JAVAText\src\列车牵引计算\计算牵引质量
        //获取文件的绝对路径
        System.out.println("绝对路径：" + file1.getParentFile());

        //获取计算机任意一个文件的绝对路径
        File file2 = new File("Mass.java");
        System.out.println("绝对路径为：" + file2.getAbsolutePath());

        //获取文件名
        File file3 = new File("D:\\JAVAText\\src\\列车牵引计算\\计算牵引质量\\Mass.java");
        System.out.println("文件名：" + file3.getName());
        //判断是否是一个文件
        System.out.println(file3.isFile()); //true
        //判断是否是一个目录
        System.out.println(file3.isDirectory()); //false

        //获取文件最后修改时间
        long longTime = file3.lastModified();
        //创建Date对象，并传入longtime，创建SimpleDateFormat方法(将毫秒化为时分秒的形式)
        Date time = new Date(longTime);
        SimpleDateFormat sp = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSSS");
        System.out.println("最后修改时间：" + sp.format(time));

        //获取文件大小
        System.out.println(file3.length());
    }
}

public class FileTExt02 {
    public static void main(String[] args) {
        File file = new File("D:\\JavaStudy\\src\\javaIoText");

        //获取当前路径下的子文件
        File[] files = file.listFiles();
        for(File flie : files){
            System.out.println(file.getAbsoluteFile());  //-->获取路径（返回此抽象路径名的绝对形式）
            //System.out.println(flie.getName()); //文件名
        }
    }
}

```

