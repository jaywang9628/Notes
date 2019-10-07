# 第一部分 File类和递归

## 第一章 File类

### 1.1 概述

`java.io.File`类是文件和目录路径名的抽象表示形式

Java把电脑中的文件和文件夹（目录）封装，我们可以使用File类对文件和文件夹进行操作

File是一个和操作系统无关的类，任何操作系统都可以使用它

#### 分隔符

`static String pathSeparator`与系统相关的路径分隔符（字符串）

`static char pathSeparator`与系统相关的路径分隔符

`static String Separator`与系统相关的默认名称分隔符（字符串）

`static char Separator`与系统相关的默认名称分隔符

- windows路径分隔符  \   文件名称分隔符   ;
- Linux路径分隔符   /    文件名称分隔符  :
- 路径不区分大小写
- windows中用两个\\来代表\，因为反斜杠是转义字符

### 1.2 构造方法

- 路径可以文件夹结尾，也可以文件结尾
- 路径可以是相对路径，也可以是绝对路径
- 路径可以存在，也可以不存在
- 创建File对象，知识把字符串路径封装成File对象，不考虑路径的真实情况

`File(String pathname)`

`File(String parent,String child)`

`File(File parent,String child)`

### 1.3 File类常用方法

#### 获取功能的方法

- `String getAbsolutePath()`返回File的绝对路径字符串
- `String getPath()`返回File的路径名称字符串（File类的toString方法调用了此方法）
- `String getName()`返回File表示的文件或者目录的名称（结尾部分，最后一个斜杠后的部分）
- `long length()`返回File表示的文件的长度

#### 判断功能的方法

- `boolean exist()` 
- `boolean isDirectory()`
- `boolean isFile()`

#### 创建删除功能的方法

- `boolean createNewFile()`创建文件（不能创建文件夹）
- `boolean delete()`删除文件或者文件夹
- `boolean mkdir()`创建目录
- `boolean mkdirs()`创建目录，包括任何不存在的父目录**（用这一种就好）**

### 1.4 目录的遍历

- ` String[] list()`返回一个String数组，表示该File目录中所有的子文件和目录
- `File[] listFiles()`返回一个File数组，表示File目录中的所有子文件或目录
  - 遍历的是构造方法中给出的目录
  - 如果目录路径不存，会抛出空指针异常
  - 如果路径不是目录，也会抛出空指针异常

## 第二章 递归

### 2.1 概述

- 当前方法中调用自己的现象
- 分类：直接递归和间接递归
- 注意事项
  - 递归一定要有条件限定，保证停下来，否则栈内存溢出
  - 就算有限定，递归次数也不能太多，否则也会栈内存溢出
  - 构造方法：禁止递归

### 2.2 递归累加求和

- 使用递归求和，需要频繁的创建、调用、销毁方法，效率低下

  可用for循环替代

```java
public static int sum (int n){
    if (n==1){
        return 0;
    }else{
        return n+sum(n-1);
    }
}
```

### 2.3 递归求阶乘

### 2.4 递归打印多级目录

```java
public static void main(String[] args){
    getAllFiles(file);
    
    public static void getAllFiles(File dir){
        File[] files = dir.listFile();
        if(f.isDirectory){
            getAllFiles(f);
        }else{
            System.out.println(f);
        }
    }
}
```



## 第三章 综合案例

### 3.1 文件搜索

```java
public static void main(String[] args){
    getAllFiles(file);
    
    public static void getAllFiles(File dir){
        File[] files = dir.listFile();
        if(f.isDirectory){
            getAllFiles(f);
        }else{
            if(f.getName().toLowerCase().endsWith(".java")){
                System.out.println(f);
            }
        }
    }
}
```

### 3.2 文件过滤器优化

- File类中两个listFiles的重载方法，方法的参数传递的就是过滤器,实现此接口的类实例可过滤文件名

  - java.io.FileFilter接口：
    - boolean accept(File pathname) 测试指定抽象路径名是否应该包含在某个路径名列表中
  - java.io.FilenameFilter接口：
    - boolean accept(File dir,String name)  测试指定抽象路径名是否应该包含在某个文件列表中

  ```java
  public static void main(String[] args){
      getAllFiles(file);
     
      public static void getAllFiles(File dir){
           //匿名内部类
          File[] files = dir.listFiles(new FileFilter(){
              @override
              public boolean accept(File pathname){
                  return pathname.isDirectory||pathname.getName().toLowerCase().endsWith(".java")
              }
          });
  /*
  listFiles()方法一共做了3件事
        1.对构造方法中传递的目录进行遍历，获取目录中的每一个文件/文件夹-->封装为File对象
          2.调用参数传递的过滤器中的accept方法
          3.把遍历得到的每一个File对象，传递给accept方法的参数pathname
   		  accept方法返回true就会把传递过去的File对象保存到File数组中，否则不会。
   */
  ```
  
  

### 3.3 Lambda优化

```java
public static void main(String[] args){
    getAllFiles(file);
   
    public static void getAllFiles(File dir){        
		 //lambda表达式
        File[] files = dir.listFiles(
            (File pathname)->return pathname.isDirectory||pathname.getName().toLowerCase().endsWith(".java");
        );
        System.out.println(files);
    }
}
```



------

# 第二部分 字节流、字符流

## 第一章 IO概述

### 1.1 什么是IO

Java中的IO操作主要是指使用java.io中的内容，进行输入输出操作。

- 输入也叫做读取数据，输出也叫做写入数据

### 1.2 IO的分类

- 输入流：（把数据从其他设备读取到内存上的流）**其他设备——>内存**
- 输出流：（把数据从内存中写出到其他设备上的流）**内存——>其他设备**
  - 格局数据类型：字节流和字符流

### 1.3 顶级父类们

|        |   输入流    |    输出流    |
| :----: | :---------: | :----------: |
| 字节流 | InputStream | OutputStream |
| 字符流 |   Reader    |    Writer    |



## 第二章 字节流

### 2.1 一切皆为字节

一切文件数据在存储时都是以二进制保存，所以底层的传输始终为二进制数据。

### 2.2 字节输出流【OutputStream】

- OutputStream抽象类是表示字节输出流所有类的超类，将指定的字节信息写出到目的地。

**`public void close()`关闭此输出流并释放系统资源**

`public void flush()`刷新此输出流并强制任何缓冲的输出字节被写出

- 写入多个字节的方法

**`public void write(byte[] bytes)`将b.length字节从指定的字节数组写入此输出流**

`public void write(byte[] b, int off, int len)`从指定的字节数组写入len字节，从偏移量off开始输出到此输出流

- `public abstract void write(int b)`将指定的字节输出流

### 2.3 FileOutputStream类

- java.io.FileOutputStream extends OutputStream  

  文件字节输出流

  作用：把内存中的数据写入到硬盘的文件中

- 构造方法

  - ` FileOutputStream(String name)`
  -  `FileOutputStream(File file)`
    - 作用
      1. 创建一个 FileOutputStream对象
      2. 根据构造方法中传递的文件/文件路径，创建一个空的文件
      3. 会把FileOutputStream对象指向创建好的对象

- 原理

  java程序->JVM->OS->OS调用写数据的方法->把数据写入到文件中

- 使用步骤

  1. 创建一个FileOutputStream对象，构造方法中传递写入数据的目的地
  2. 调用FileOutputStream对象的write方法，写入数据
  3. 释放资源，使用close方法（流的使用会占用内存，完毕要清空来提高系统运行效率）

- 续写：通过构造方法

  - `FileOutputStream(String name ,boolean append)`
  - `FileOutputStream(File file ,boolean append)`

- **注意**

  1. 写数据的时候，会把10进制的97转为2进制数

  2. 任意的文本编辑器，在打开文件时，都会查询编码表，把字节转换为字符表示

     1~127：查询ASCII码

     其他：查询系统默认码（中文系统GBK)

  3. 写入字符，可以使用String类中的`byte[] getBytes()`方法吧字符串转化为字节数组

  4. 换行符：

     window:\r\n

     linux:/n

     mac:/r

  ```java
  //续写
  FileOutputStream fos = new FileOutputStream("\\wjj\\a.txt",true);
  for (int i = 1;i<=10;i++){
      fos.write("Hello\r\n".getBytes)
  }
  ```

  

### 2.4 字节输入流【InputStream】

java.io.InputStream抽象类是所有表示字节输入流的超类。

- `public void close()`关闭输入流并释放相关系统资源
- `public abstract int read()`从输入流读取数据的下一个字节
- `public int read(byte[] bytes)`从输入流中读取一些字节数，并将他们存储到字节数组中

### 2.5 FileInputStream类

- java.io.FileInputStream extends InputStream  

  文件字节输入流

  作用：把硬盘的文件中的数据读取到内存中使用

- 构造方法：

  `FileInputStream(String name)`

  `FileInputStream(File file)`

- 使用步骤

  1. 创建FileInputStream对象，构造方法中绑定要读取的数据源
  2. 使用FileInputStream对象中的方法read，读取数据
  3. 释放资源

- 注意

  1. int read()读取文件中的一个字节并返回，读取到文件的末尾返回-1
  2. 使用while((len = fis.read())!=-1)来读字节
  3. 可以用(char)来强制转化字节为字符

  ```java
  FileInputStream fis = new FileInputStream("wjj\\c.txt");
  while((len = fis.read())!=-1){
      System.out.println((char)len);
  }
  fis.close();
  ```

  

- 读取多个字节

  - `int read(byte[] bytes) `从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
  - 数组的作用：存储读取到的多个字节，起缓冲作用。长度一般定义为1024(1kb)或者1024的整数倍
  - 方法的返回值int：每次返回读取的有效字节个数，返回值为-1时则读到末尾了

  ```java
  FileInputStream fis = new FileInputStream("wjj\\c.txt");
  byte [] bytes = new bytes[1024];
  len = fis.read(bytes);
  System.out.println(len);
  System.out.println(new String(bytes));//new一个String对象来字符显示
  fis.close();//关闭输入流
  ```

### 2.6 字节流练习：图片复制

## 第三章 字符流

### 3.1 字符输入流【Reader】

### 3.2 FileReader类

- 构造方法

- 读取字符数据

### 3.3 字符输出流【Writer】

### 3.4 FileWriter类

- 构造方法

- 基本写出数据

- 关闭和刷新

- 写出其他数据

## 第四章 IO异常的处理

### 4.1 

### 4.2

### 4.3

### 4.4

### 4.5