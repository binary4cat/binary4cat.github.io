---
title: Java基础：String&StringBuilder&StringBuffer
date: 2017-05-20 23:58:14
categories: Java
tags: Java基础
permalink:
description: Java基础：String&StringBuilder&StringBuffer
photos: http://ww1.sinaimg.cn/large/c55a7aeely1fmbwbz5s71j20et08cjra.jpg
---		
### String类的概念和不变性
- String类的概念和不变性
	- String类
		- API中的String类的描述，发现String 类代表字符串
		- Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。
		- 字符串是常量,在创建之后不能更改
		- 其实就是说一旦这个字符串确定了，那么就会在内存区域中就生成了这个字符串。字符串本身不能改变，但str变量中记录的地址值是可以改变的。
		- 源码分析,String类底层采用的是字符数组:
    
        ```Java
		private final char value[]
		//private 修饰说明value只能在String类内部使用,而且又没有提供get方法,所以外部无法获取value数组,就无法改变数组中元素的值
		//final修饰说明value是常量,一旦创建,就不能被改变,value一旦被初始化成某个数组,将永远指向这个数组,不可能再指向其它的数组了
		```		
```Java
			/*
			 *   String类特点:
			 *     一切都是对象,字符串事物 "" 也是对象
			 *     类是描述事物,String类,描述字符串对象的类
			 *     所有的 "" 都是String类的对象
			 *     
			 *     字符串是一个常量,一旦创建,不能改变
			 */
			public class StringDemo {
				public static void main(String[] args) {
					//引用变量str执行内存变化
					//定义好的字符串对象,不变
					String str = "itcast";
					System.out.println(str);
					str = "itheima";
					System.out.println(str);
					
					
				}
			}
```			
###07String类创建方式和比较
	* A: String类创建方式和比较
		* a: 创建对象的数量比较
			* String s3 = "abc";
				* 在内存中只有一个对象。这个对象在字符串常量池中
			* String s4 = new String("abc");
				* 在内存中有两个对象。一个new的对象在堆中，一个字符串本身对象，在字符串常量池中
		* b: 案例代码
			public class StringDemo2 {
				public static void main(String[] args) {
					//字符串定义方式2个, 直接=  使用String类的构造方法
					String str1 = new String("abc");
					String str2 = "abc";
					System.out.println(str1);
					System.out.println(str2);
					
					System.out.println(str1==str2);//引用数据类型,比较对象的地址 false
					System.out.println(str1.equals(str2));//true
				}
			}
						
###08String类构造方法
	* A: String类构造方法
		* a: 常见构造方法
			* public String():空构造
			* public String(byte[] bytes):把字节数组转成字符串
			* public String(byte[] bytes,int index,int length):把字节数组的一部分转成字符串			
			* public String(String original):把字符串常量值转成字符串
		* b: 案例代码
			public class StringDemo3 {
				public static void main(String[] args) {
					function_1();
				}
				/*
				 *  定义方法,String类的构造方法
				 *  String(byte[] bytes)  传递字节数组
				 *  字节数组转成字符串
				 *  通过使用平台的默认字符集解码指定的 byte 数组，构造一个新的 String。
				 *  平台 : 机器操作系统
				 *  默认字符集: 操作系统中的默认编码表, 默认编码表GBK
				 *  将字节数组中的每个字节,查询了编码表,得到的结果
				 *  字节是负数,汉字的字节编码就是负数, 默认编码表 ,一个汉字采用2个字节表示
				 *  
				 *  String(byte[] bytes, int offset, int length) 传递字节数组
				 *  字节数组的一部分转成字符串
				 *  offset 数组的起始的索引
				 *  length 个数,转几个   , 不是结束的索引
				 */
				public static void function(){
					byte[] bytes = {97,98,99,100};
					//调用String类的构造方法,传递字节数组
					String s = new String(bytes);
					System.out.println(s);
					
					byte[] bytes1 ={65,66,67,68,69};
					//调用String构造方法,传递数组,传递2个int值
					String s1 = new String(bytes1,1,3);
					System.out.println(s1);
				}
			}


###09String类构造方法_2
	* A: String类构造方法
		* a: 常见构造方法
			* public String(char[] value):把字符数组转成字符串
			* public String(char[] value,int index,int count):把字符数组的一部分转成字符串
	* B: 案例代码
		 /*
		  *  String类构造方法
		  *  String类的构造方法,重载形式
		  * 
		  */
		public class StringDemo3 {
			public static void main(String[] args) {
				function_1();
			}
			/*
			 * String(char[] value) 传递字符数组
			 * 将字符数组,转成字符串, 字符数组的参数,不查询编码表
			 * 
			 * String(char[] value, int offset, int count) 传递字符数组
			 * 将字符数组的一部分转成字符串
			 * offset  数组开始索引
			 * count   个数
			 */
			public static void function_1(){
				char[] ch = {'a','b','c','d','e','f'};
				//调用String构造方法,传递字符数组
				String s = new String(ch);
				System.out.println(s);
				
				String s1 = new String(ch,1,4);
				System.out.println(s1);
			}
		}




		
###10String类的其他方法			
	* A：String类的其他方法
		* a: 方法介绍
			* int length(): 返回字符串的长度
			* String substring(int beginIndex,int endIndex): 获取字符串的一部分
			* String substring(int beginIndex): 获取字符串的一部分
			* boolean startsWith(String prefix): 判断一个字符串是不是另一个字符串的前缀,开头
			* boolean endsWith(String prefix): 判断一个字符串是不是另一个字符串的后缀,结尾
			* boolean contains (String s): 判断一个字符串中,是否包含另一个字符串
			* int indexOf(char ch):  查找一个字符,在字符串中第一次出现的索引,被查找的字符不存在,返回-1
			* byte[] getBytes(): 将字符串转成字节数组,此功能和String构造方法相反,byte数组相关的功能,查询编码表
			* char[] toCharArray(): 将字符串转成字符数组,功能和构造方法相反
			* boolean equals(Object obj): 方法传递字符串,判断字符串中的字符是否完全相同,如果完全相同返回true
			* boolean equalsIgnoreCase(String s): 传递字符串,判断字符串中的字符是否相同,忽略大小写			
			
		* b: 案例代码
		
			public class StringDemo4 {
				public static void main(String[] args) {
					function_9();
				}
				/*
				 *  boolean equals(Object obj)
				 *  方法传递字符串,判断字符串中的字符是否完全相同,如果完全相同返回true
				 *  
				 *  boolean equalsIgnoreCase(String s)
				 *  传递字符串,判断字符串中的字符是否相同,忽略大小写
				 */
				public static void function_9(){
					String str1 = "Abc";
					String str2 = "abc";
					//分别调用equals和equalsIgnoreCase
					boolean b1 = str1.equals(str2);
					boolean b2 = str1.equalsIgnoreCase(str2);
					System.out.println(b1);
					System.out.println(b2);
				}
				
				/*
				 * char[] toCharArray() 将字符串转成字符数组
				 * 功能和构造方法相反
				 */
				public static void function_8(){
					String str = "itcast";
					//调用String类的方法toCharArray()
					char[] ch = str.toCharArray();
					for(int i = 0 ; i < ch.length ; i++){
						System.out.println(ch[i]);
					}
				}
				
				/*
				 *  byte[] getBytes() 将字符串转成字节数组
				 *  此功能和String构造方法相反
				 *  byte数组相关的功能,查询编码表
				 */
				public static void function_7(){
					String str = "abc";
					//调用String类方法getBytes字符串转成字节数组
					byte[] bytes = str.getBytes();
					for(int i = 0 ; i < bytes.length ; i++){
						System.out.println(bytes[i]);
					}
				}
				
				/*
				 *  int indexOf(char ch)
				 *  查找一个字符,在字符串中第一次出现的索引
				 *  被查找的字符不存在,返回-1
				 */
				public static void function_6(){
					String str = "itcast.cn";
					//调用String类的方法indexOf
					int index = str.indexOf('x');
					System.out.println(index);
				}
				
				/*
				 *  boolean contains (String s)
				 *  判断一个字符串中,是否包含另一个字符串
				 */
				public static void function_5(){
					String str = "itcast.cn";
					//调用String类的方法contains
					boolean b =str.contains("ac");
					System.out.println(b);
				}
				
				/*
				 * boolean endsWith(String prefix)
				 * 判断一个字符串是不是另一个字符串的后缀,结尾
				 * Demo.java
				 *     .java
				 */
				public static void function_4(){
					String str = "Demo.java";
					//调用String类方法endsWith
					boolean b = str.endsWith(".java");
					System.out.println(b);
				}
				
				/*
				 * boolean startsWith(String prefix)  
				 * 判断一个字符串是不是另一个字符串的前缀,开头
				 * howareyou
				 * hOw
				 */
				  public static void function_3(){
					  String str = "howareyou";
					  //调用String类的方法startsWith
					  boolean b = str.startsWith("hOw");
					  System.out.println(b);
				  }
				
				/*
				 *  String substring(int beginIndex,int endIndex) 获取字符串的一部分
				 *  返回新的字符串
				 *  包含头,不包含尾巴
				 *  
				 *  String substring(int beginIndex)获取字符串的一部分
				 *  包含头,后面的字符全要
				 */
				public static void function_2(){
					String str = "howareyou";
					//调用String类方法substring获取字符串一部分
					str= str.substring(1, 5);
					System.out.println(str);
					
					String str2 = "HelloWorld";
					str2 = str2.substring(1);
					System.out.println(str2);
				}
				
				/*
				 *  int length() 返回字符串的长度
				 *  包含多少个字符
				 */
				public static void function(){
					String str = "cfxdf#$REFewfrt54GT";
					//调用String类方法length,获取字符串长度
					int length = str.length();
					System.out.println(length);
				}
			}
				
###11String类练习
	* A: 获取指定字符串中，大写字母、小写字母、数字的个数
		* a: 题目分析
			* 为了统计大写字母、小写字母、数字的个数。创建3个计数的变量。
			* 为了获取到字符串中的每个字符，进行字符串的遍历，得到每个字符。
			* 对得到的字符进行判断，如果该字符为大写字母，则大写字母个数+1；如果该字符为小写字母，则小写字母个数+1；如果该字符为数字，则数字个数+1。
			* 显示大写字母、小写字母、数字的个数

		* b: 解题步骤
			* 略
		* 案例代码
			public class StringTest {
				public static void main(String[] args) {
					getCount("A%A3eBr1FFy");					
				}
				
				/*
				 * 获取指定字符串中，大写字母、小写字母、数字的个数。
				 * 思想:
				 *   1. 计数器,就是int变量,满足一个条件 ++
				 *   2. 遍历字符串, 长度方法length() + charAt() 遍历
				 *   3. 字符判断是大写,是小写,还是数字
				 */
				public static void getCount(String str){
					//定义三个变量,计数
					int upper = 0;
					int lower = 0;
					int digit = 0;
					//对字符串遍历
					for(int i = 0 ; i < str.length() ; i++){
						//String方法charAt,索引,获取字符
						char c = str.charAt(i);
						//利用编码表 65-90  97-122  48-57
						if(c >='A' && c <=90){
							upper++;
						}else if( c >= 97 && c <= 122){
							lower++;
						}else if( c >= 48 && c <='9'){
							digit++;
						}
					}
					System.out.println(upper);
					System.out.println(lower);
					System.out.println(digit);
				}
			}

###12String类练习_2
	* A: 将字符串中，第一个字母转换成大写，其他字母转换成小写，并打印改变后的字符串。
		* a: 题目分析
			* 把字符串分为两个部分，第一部分为字符串中第一个字母，第二部分为剩下的字符串。
			* 把第一部分字符串转换成大写字母，把第二部分字符串转换成小写字母
			* 把两部分字符串连接在一起，得到一个完整的字符串
		* b: 解题步骤
			* 略
		* C: 案例代码
			public class StringTest {
				public static void main(String[] args) {
					
					System.out.println(toConvert("aBc5%4dEF"));
					
				}
				
				/*
				 *  将字符串的首字母转成大写,其他内容转成小写
				 *  思想:
				 *    获取首字母, charAt(0)  substring(0,1)
				 *    转成大写 toUpperCase()
				 *    
				 *    获取剩余字符串, substring(1)  toLowerCase()
				 */
				public static String toConvert(String str){
					//定义变量,保存首字母,和剩余字符
					String first = str.substring(0,1);
					String after = str.substring(1);
					//调用String类方法,大写,小写转换
					first = first.toUpperCase();
					after = after.toLowerCase();
					return first+after;
				}
			}



###13String类练习_3
	* A: 查询大字符串中，出现指定小字符串的次数
		* a: 题目分析
			* 在大串中，查找小串出现的位置，出现了就次数+1
			* 在上次小串出现位置的后面继续查找，需要更改大串的内容为上次未查询到的字符串。
			* 回到第一步，继续查找小串出现的位置，直到大串中查询不到小串为止
		* b: 解题步骤
			* 略
		* C: 案例代码	
			package cn.itcast.demo02;

			public class StringTest {
				public static void main(String[] args) {		
					System.out.println(getStringCount("hellojava,nijavahaojava,javazhenbang", "java"));
				}
				/*
				 *  获取一个字符串中,另一个字符串出现的次数
				 *  思想:
				 *    1. indexOf到字符串中到第一次出现的索引
				 *    2. 找到的索引+被找字符串长度,截取字符串
				 *    3. 计数器++
				 */
				public static int getStringCount(String str, String key){
					//定义计数器
					int count = 0;
					//定义变量,保存indexOf查找后的索引的结果
					int index = 0;
					//开始循环找,条件,indexOf==-1 字符串没有了
					while(( index = str.indexOf(key) )!= -1){
						count++;
						//获取到的索引,和字符串长度求和,截取字符串
						str = str.substring(index+key.length());
					}
					return count;
				}
			}


======================================================================第三节课开始=========================================================
				
###14StringBuffer特点可变字符数组
	* A:StringBuffer类概述
		* 通过JDK提供的API，查看StringBuffer类的说明
		* 线程安全的可变字符序列 
		* 底层采用字符数组实现,初始容量为16
	* B:StringBuffer和String的区别
		* String是一个不可变的字符序列
		* StringBuffer是一个可变的字符序列

###15StringBuffer类的方法
	* A: StringBuffer类的方法
		* a: 方法介绍
			* StringBuffer append(), 将任意类型的数据,添加缓冲区
				*  append 返回值,写return this
				*  调用者是谁,返回值就是谁
			* delete(int start,int end): 删除缓冲区中字符
				*  开始索引包含,结尾索引不包含
			* insert(int index, 任意类型): 将任意类型数据,插入到缓冲区的指定索引上
			* replace(int start,int end, String str): 将指定的索引范围内的所有字符,替换成新的字符串
			* reverse(): 将缓冲区中的字符反转
			* String toString(): 继承Object,重写toString()
				*   将缓冲区中的所有字符,变成字符串
		* b: 案例代码
			public class StringBufferDemo {
				public static void main(String[] args) {
					function_5();
				}
				/*
				 *  StringBuffer类的方法
				 *   String toString() 继承Object,重写toString()
				 *   将缓冲区中的所有字符,变成字符串
				 */
				public static void function_5(){
					StringBuffer buffer = new StringBuffer();
					buffer.append("abcdef");
					buffer.append(12345);
					
					//将可变的字符串缓冲区对象,变成了不可变String对象
					String s = buffer.toString();
					System.out.println(s);
				}
				
				/*
				 *  StringBuffer类的方法
				 *    reverse() 将缓冲区中的字符反转
				 */
				public static void function_4(){
					StringBuffer buffer = new StringBuffer();
					buffer.append("abcdef");
					
					buffer.reverse();
					
					System.out.println(buffer);
				}
				
				/*
				 *  StringBuffer类方法
				 *    replace(int start,int end, String str)
				 *    将指定的索引范围内的所有字符,替换成新的字符串
				 */
				public static void function_3(){
					StringBuffer buffer = new StringBuffer();
					buffer.append("abcdef");
					
					buffer.replace(1, 4, "Q");
					
					System.out.println(buffer);
				}
				
				/*
				 *  StringBuffer类方法 insert
				 *    insert(int index, 任意类型)
				 *  将任意类型数据,插入到缓冲区的指定索引上
				 */
				 public static void function_2(){
					 StringBuffer buffer = new StringBuffer();
					 buffer.append("abcdef");	 
					 
					 buffer.insert(3, 9.5);
					 System.out.println(buffer);
				 }
				
				/*
				 * StringBuffer类方法
				 *   delete(int start,int end) 删除缓冲区中字符
				 *   开始索引包含,结尾索引不包含
				 */
				public static void function_1(){
					StringBuffer buffer = new StringBuffer();
					buffer.append("abcdef");
					
					buffer.delete(1,5);
					System.out.println(buffer);
				}
				
				/*
				 *  StringBuffer类方法
				 *   StringBuffer append, 将任意类型的数据,添加缓冲区
				 *   append 返回值,写return this
				 *   调用者是谁,返回值就是谁
				 */
				public static void function(){
					StringBuffer buffer = new StringBuffer();
					//调用StringBuffer方法append向缓冲区追加内容
					buffer.append(6).append(false).append('a').append(1.5);
					System.out.println(buffer);
				}
			}

		
###16StringBuilder类
	* A:StringBuilder的概述
		* 通过查看API了解一下StringBuilder类
	* B:面试题
		* String,StringBuffer,StringBuilder的区别
			* StringBuffer和StringBuilder的区别
				* StringBuffer是jdk1.0版本的,是线程安全的,效率低
				* StringBuilder是jdk1.5版本的,是线程不安全的,效率高

			* String和StringBuffer,StringBuilder的区别
				* String是一个不可变的字符序列
				* StringBuffer,StringBuilder是可变的字符序列
	


		
###17StringBuffer类案例拼接数组
	* A: StringBuffer类案例拼接数组
		* a: 题目分析
			* 定义StringBuffer对象
			* 遍历数组,按照格式要求拼接处新的字符串,追加到StringBuffer容器中
			* 将StringBuffer中的内容以String的形式返回
		* b: 解题步骤
			* 略
		* C: 案例代码	
			public class StringBufferTest {
				public static void main(String[] args) {
					int[] arr = {4,1,4,56,7,8,76};
					System.out.println(toString(arr));
				}
			   /*
				* int[] arr = {34,12,89,68};将一个int[]中元素转成字符串 
				* 格式 [34,12,89,68]
				* String s = "["
				* 数组遍历
				*   s+= arr[i];
				*  s+"]"
				*  StringBuffer实现,节约内存空间, String + 在缓冲区中,append方法
				*/
				public static String toString(int[] arr){
					//创建字符串缓冲区
					StringBuffer buffer = new StringBuffer();
					buffer.append("[");
					//数组遍历
					for(int i = 0 ; i < arr.length;i++){
						//判断是不是数组的最后一个元素
						if(i == arr.length-1){
							buffer.append(arr[i]).append("]");
						}else{
							buffer.append(arr[i]).append(",");
						}
					}
					return buffer.toString();
				}
			}
###18总结
* 把今天的知识点总结一遍。