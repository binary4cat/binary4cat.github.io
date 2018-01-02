---
title: Java基础：文件操作File类
date: 2017-06-25 22:41:45
categories: Java
tags: Java基础
permalink: 
description: Java文件操作File类介绍
photos: http://ww1.sinaimg.cn/large/c55a7aeely1fmswz37pb7j20b408cweg.jpg
---

### File类概述
- `File`类是文件和目录路径名的抽象表示形式
- Java中把文件或者文件夹都封装成File对象
- 使用`File`类可以操作硬盘上的文件、文件夹
			
### File类静态的成员变量
- `pathSeparator`
    - 分隔连续多个路径字符串的分隔符(例如Windows中环境变量的路径之间的分隔符是`;`号，而Linux中是`:`号)，根据系统获取，屏蔽系统间的差异。
- `separator`
    - 分隔同一个路径中的不同目录的分隔符(例如Windows中为`\`，Linux中为`/`)


### File类构造方法
- `File(String pathname)`
    - 通过将给定路径名字符串转换为一个File对象,之后可以使用File中的方法
	- windows中的路径或文件名不区分大小写

- `File(String parent, String child)`
    - 将传递的两个路径字符串合并成一个路径，并返回`File`对象。


### File类创建文件
- public boolean createNewFile()
    - 创建文件，如果指定目录下有同名文件，则不会创建。

```java
public static void main(String[] args) throws IOException{
	File file=new File("C:test.txt");
	boolean bool= file.createNewFile();
	System.out.println(bool);  //已经存在的文件再创建，就返回false
}
```


### File类创建目录
- `public boolean mkdir()`
    - 创建文件夹，只能创建一级文件夹，也就是说要创建的文件夹的父文件夹必须存在，否则就会创建失败。
- `public boolean mkdirs()`
    - 创建文件夹，可以创建多级文件夹，父文件夹不存在则会按级创建。

```java
public static void main(String[] args) throws IOException{
	File file=new File("C:\\Users\\Desktop\\test\\123\\test.txt");   
	/*
	* 创建目录会把所有的字符当作文件夹名字，
	* 所以这里的test.txt并不会创建一个文件，
	* 而是创建成一个名为test.txt的文件夹
	*/
	boolean bool= file.mkdirs();
	System.out.println(bool);  
}
```


### File类删除功能
- `public boolean delete()`:删除文件或者文件夹
	- 该删除方法直接从磁盘中删除，不会进入回收站。
		
### File类获取功能			
- `String getName()`: 返回的是路径中最后一级的数据，或者是文件名，或者是最后一级文件夹名
- `long length()`: 返回路径指向的文件的字节数，找不到文件则返回0
- `String getAbsolutePath()`: 获取绝对路径,返回String类型
- `File getAbsoluteFile()` : 获取绝对路径,返回File对象
- `String getParent()`: 获取父路径,返回String对象
- `File getParentFile()`: 获取父路径,返回File对象



### File类判断功能
- `boolean exists()`: 判断File构造方法中封装路径是否存在
	- 存在返回`true`,不存在返回`false`
- `boolean isDirectory()`: 判断`File`构造方法中封装的路径是不是文件夹
	- 如果是文件夹,返回`true`,不是文件返回`false`
- `boolean isFile()`: 判断`File`构造方法中封装的路径是不是文件
	- 如果是文件,返回`true`,不是文件返回`false`



### File类list获取功能
- `String[] list()`：获取到File构造方法中传递的路径下的所有文件和文件夹名称
	- 如果路径写的是文件，那么就会返回null，所以这里应该传入一个文件夹的路径
	- 返回的是该文件夹下的文件名
- `File[] listFiles()`：获取到File构造方法中传递的路径下的所有文件和文件夹的File对象集合。
```java
public static void main(String[] args) throws IOException{
	File file=new File("C:\\Users\\Desktop\\test\\123");
	File[] bool= file.listFiles();
	for (File string : bool) {
	 	System.out.println(string.getName());
	}
}
```
- `static File[] listRoots()`: 获取当前文件系统的跟目录
	- 获取的是文件系统的盘符
```java
public static void main(String[] args) throws IOException{
	File[] bool= File.listRoots();
	for (File string : bool) {
	System.out.println(string);  //输出 C:\  D:\  等盘符
	}
}
```


### 文件过滤器
- `File[] listFiles(FileFilter filter)` :可以利用自定义的过滤器实现查找指定的文件。
- 实现`FileFilter`接口，自定义过滤器
```java
public class TestFilter implements FileFilter {
	@Override
	public boolean accept(File pathname) {
		return f.getName().endsWith(".jpg");
	}
}
```
- 调用自定义过滤器
```java
public static void main(String[] args) throws IOException{
	File file=new File("C:\\Users\\Desktop\\test\\123");
	File[] bool= file.listFiles(new TestFilter());   //获取指定文件夹下的后缀为.jpg的文件
	for (File string : bool) {
	 	System.out.println(string);
	}
}
```


### 递归遍历全目录
```java
public class Test {
	public static void main(String[] args) throws IOException {
		File file = new File("C:\\Users\\Desktop\\test");
		getAll(file);
	}
	public static void getAll(File file) {
		File[] files = file.listFiles();
		for (File f : files) {
			if (f.isDirectory()) {
				getAll(f);
			} else {
				System.out.println(f);
			}
		}
	}
}
```