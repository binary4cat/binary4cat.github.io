---
title: Java基础：字节流&字符流操作
date: 2017-06-29 19:50:24
categories: Java
tags: Java基础
permalink:
description: IO操作字节流和字符流输入输出
photos: http://ww1.sinaimg.cn/large/c55a7aeely1fmswz37pb7j20b408cweg.jpg
---
<!-- TOC -->

- [概念](#概念)
    - [流按操作类型分为两种：](#流按操作类型分为两种)
- [字节流实现类](#字节流实现类)
    - [`FileOutputStream`](#fileoutputstream)
        - [写入单个字节](#写入单个字节)
        - [写字节数组](#写字节数组)
    - [`FileInputStream`](#fileinputstream)
        - [读取单个字节](#读取单个字节)
        - [读取字节数组](#读取字节数组)
- [字符流实现类](#字符流实现类)
    - [`FileWriter`](#filewriter)
        - [主要方法](#主要方法)
    - [`FileReader`](#filereader)
        - [`flush`方法和`close`方法区别](#flush方法和close方法区别)

<!-- /TOC -->
## 概念
### 流按操作类型分为两种：
- 字节流 : 字节流可以操作任何数据,因为在计算机中任何数据都是以字节的形式存储的
	- `OutputStream` :字节输出流(抽象类)
	- `InputStream` :字符输入流(抽象类)
- 字符流 : 字符流只能操作纯字符数据。
	- `Reader`:字符输入流(抽象类)
	- `Writer`:字符输出流(抽象类)
<!--more-->
## 字节流实现类
###	`FileOutputStream`
- 构造方法：
	- `FileOutputStream(File file)`
		- 创建一个向指定`File`对象表示的文件中写入数据的文件输出流。(默认以覆盖方式写入)
	- `FileOutputStream(File file, boolean append)` 
		- 创建一个向指定`File`对象表示的文件中写入数据的文件输出流，并指定是否以追加的方式写入。
	- `FileOutputStream(String name)` 
		- 创建一个向具有指定名称(全路径)的文件中写入数据的输出文件流。(默认以覆盖方式写入)
	- `FileOutputStream(String name, boolean append)` 
		- 创建一个向具有指定`name`的文件中写入数据的输出文件流，并指定是否以追加的方式写入。

#### 写入单个字节
- - `void write(int b)`:写入单个字节。
```java
public static void main(String[] args) throws IOException {
	FileOutputStream fileOutputStream=new FileOutputStream("C:\\txt1.txt");
	fileOutputStream.write('h');  //write方法虽然接受的是int类型的参数，但是协定是写入一个字节。
	fileOutputStream.write('a');
	fileOutputStream.write('i');
	fileOutputStream.write(106);  //参数是int类型的，会默认转换成ASCII码对应的字符。
	fileOutputStream.write(100);
	fileOutputStream.close();
	// 打开文件可以看到写入的字符串“haijd”
}
```

#### 写字节数组
- `void write(byte[] b)`： 将指定的字节数组写入输出流
- `void write(byte[] b, int off, int len)` ：将指定的字节数组，从off处开始写入len个到输出流

```java
public static void main(String[] args) throws IOException {
	FileOutputStream fileOutputStream=new FileOutputStream("C:\\txt1.txt");
	byte[] b= {104,97,105,'j','d'}; 
	fileOutputStream.write(b);
	fileOutputStream.write('\n');
		
	byte[] b1="haijd".getBytes();
	fileOutputStream.write(b1);
	fileOutputStream.write('\n');
		
	byte[] b3= {104,97,105,'j','d'};
	fileOutputStream.write(b3, 3, 2);
	fileOutputStream.write('\n');
		
	fileOutputStream.close();
	/*
	* 输出：
	* haijd
	* haijd
	* jd
	*/
}
```
					
### `FileInputStream`
- 构造函数：
	- `FileInputStream(String name)`:创建一个从指定文件名(全路径)的文件中读取数据的输入流。
	- `FileInputStream(File file)`:创建一个`File`对象所表示的文件的数据读取输入流。

#### 读取单个字节
- `int read()`：从输入流中读取下一个字节，如果返回`-1`则表示文件读取结束。

```java
public static void main(String[] args) throws IOException {
	FileInputStream fileInputStream=new FileInputStream("C:\\txt1.txt");
	int b=0;
	while ((b=fileInputStream.read())!=-1) {
		System.out.println((char)b);   //输出 h a i j d 
	}
	fileInputStream.close();
}
```

#### 读取字节数组
- `int read(byte[] b)`  
	- 参数：从输入流中读取`b.length`长度的数据，暂存到`b`中。
	- 返回值：读入`b`中的字节数的长度，如果没有更多数据(文件已读完)，则返回-1.
- `int read(byte[] b, int off, int len)` 
	- 参数：
		- `b`:暂存读取数据的字节数组。
		- `off`:指定数组`b`的其实偏移量(也就是从数组b的哪个索引开始，存储数据)
		- `len`:存储到数组`b`的数据的长度。
	- 返回值：读入`b`中的字节数的长度，如果没有更多数据(文件已读完)，则返回-1.
	
```java
public static void main(String[] args) throws IOException {
	FileInputStream fileInputStream = new FileInputStream("C:\\txt1.txt");
	byte[] bs = new byte[2];
	int b = 0;
	while ((b = fileInputStream.read(bs)) != -1) {
		System.out.println(new String(bs));  //输出：ha ij dj
	}
	fileInputStream.close();
	//最后一个输出有两个字符，是因为没有清空byte数组，
	//每次都是覆盖操作，所以最后一次只有一个字符，“ij”中的j成了漏网之鱼，出现在了第三次读取中。
}
```

## 字符流实现类

### `FileWriter`
#### 主要方法
- `FileWriter`用的主要是父类`OutputStreamWriter`的方法，
- `void write(int c)`:写入单个字符
- `void write(String str)`:写入字符串(该方法是它的顶级父类`Writer`的方法)
- `void write(String str, int off, int len)`:写入字符串的某一部分
- `void write(char cbuf[], int off, int len)`：写入字符数组的一部分

```java
public static void main(String[] args) throws IOException {
	FileWriter fWriter=new FileWriter("C:\\txt1.txt");
	fWriter.write(100);  //这里依然将数字当作ASCII码
	fWriter.write("123");
	fWriter.write("456", 0, 3);
	fWriter.write("789".toCharArray(), 1, 2);
	fWriter.flush();    //字符流必须调用该方法刷新
	fWriter.close();
}
//最终写入：d12345689
```
### `FileReader`
- `FileReader`使用的主要是父类的`InputStreamReader`的方法


#### `flush`方法和`close`方法区别
- `flush()`方法
	- 用来刷新缓冲区的,刷新后可以再次写出,只有字符流才需要刷新
- `close()`方法
	- 用来关闭流释放资源的的,如果是带缓冲区的流对象的close()方法,不但会关闭流,还会再关闭流之前刷新缓冲区,关闭后不能再写出 

