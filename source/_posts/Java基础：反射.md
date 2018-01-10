---
title: Java基础：反射
date: 2017-07-09 22:58:06
categories: Java
tags: Java基础
permalink:
description: Java基础知识：反射和类加载器
photos: http://ww1.sinaimg.cn/large/c55a7aeely1fmswz37pb7j20b408cweg.jpg
---
<!-- TOC -->

- [反射](#反射)
    - [反射定义](#反射定义)
    - [Class类](#class类)
    - [通过反射获取无参构造函数并使用](#通过反射获取无参构造函数并使用)
    - [通过反射获取有参构造函数并使用](#通过反射获取有参构造函数并使用)
    - [通过反射获取私有构造函数并使用](#通过反射获取私有构造函数并使用)
    - [反射获取成员变量](#反射获取成员变量)
    - [反射修改成员变量(Field)的值](#反射修改成员变量field的值)
    - [反射获取空参数成员方法](#反射获取空参数成员方法)
        - [使用Method方法对象](#使用method方法对象)
    - [反射获取有参数成员方法](#反射获取有参数成员方法)
        - [使用Method方法对象](#使用method方法对象-1)
    - [反射泛型](#反射泛型)

<!-- /TOC -->
## 反射
### 反射定义
- Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取信息以及动态调用对象的方法的功能称为Java语言的反射机制。
- 要想解剖一个类,必须先要获取到该类的字节码文件对象。而解剖使用的就是Class类中的方法.所以先要获取到每一个字节码文件对应的Class类型的对象。

<!--more-->
### Class类
- Class类及Class对象：
	- 要想解剖一个类，必须先了解Class对象。
	- 阅读API的`Class`类得知，`Class`没有公共构造方法。`Class`对象是在加载类时由JVM以及通过调用类加载器中的`defineClass`方法自动构造的。
- 获得Class对象
	1. 通过Object类中的getClass()方法
    ```java
    User user = new User();
	Class c1 = user.getClass();
    ```
    2. 通过`类名.class`获取到字节码文件对象（任意数据类型都具备一个class静态属性）。
    ```java
    Class c2 = User.class;
    ```
    3. 通过Class类中的方法（将类名作为字符串传递给Class类中的静态方法forName即可）。
	```java
    Class c3 = Class.forName("User");
    ```


### 通过反射获取无参构造函数并使用
- 获取对象的无参构造函数
	`public Constructor<?>[] getConstructors()`:获取所有的public构造函数
	`public Constructor<T> getConstructor()`：获取无参构造函数 
- 运行无参构造函数
	`public T newInstance()`
```java
Class c=Class.forName("Demo.Person");
Constructor constructor=c.getConstructor();
Object object=constructor.newInstance();
System.out.println(object.toString());
//也可以直接调用对象的空参构造函数，前提是对象必须声明一个空参构造函数
Class c=Class.forName("Demo.Person");
Object object=c.newInstance();
System.out.println(object.toString());
```

### 通过反射获取有参构造函数并使用
- 获取有参构造函数
	`public Constructor<T> getConstructor(Class<?>... parameterTypes)`：传入匹配的参数类型的class对象，例如String.class、int.class
- 运行有参构造函数
	`public T newInstance(Object... initargs)` :传入对应的初始化参数值即可。
```java
Class c=Class.forName("Demo.Person");
Constructor constructor=c.getConstructor(String.class,int.class);
Object object=constructor.newInstance("zhangsan",22);
System.out.println(object.toString());
```

### 通过反射获取私有构造函数并使用
- 获取私有构造函数
	`public Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)` 
	`public Constructor<?>[] getDeclaredConstructors()`：获取所有的构造函数，包括私有构造函数
- 运行私有构造方法
	`public void setAccessible(boolean flag)`：设置标识(取消运行时期的权限检查，设置为true时保证我们可以运行获取的私有构造函数)
	`public T newInstance(Object... initargs)` 
```java
Class c=Class.forName("Demo.Person");
Constructor con=c.getDeclaredConstructor(int.class,String.class);
con.setAccessible(true);
Object object=con.newInstance(22,"zhangsan");
System.out.println(object.toString());
```

### 反射获取成员变量
- `public Field getField(String name)` :返回一个 Field 对象，它反映此 Class 对象所表示的类或接口的指定公共成员字段。 
- `public Field[] getFields()` :返回一个包含某些 Field 对象的数组，这些对象反映此 Class 对象所表示的类或接口的所有可访问公共字段。 
- `public Field getDeclaredField(String name)` :返回一个 Field 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明字段。 
- `public Field[] getDeclaredFields()`:返回 Field 对象的一个数组，这些对象反映此 Class 对象所表示的类或接口所声明的所有字段。 
```java
public static void main(String[] args) throws Exception  {
	Class class1=Class.forName("Demo.Person");
	Field[] fields=class1.getFields();
	for (Field field : fields) {
		System.out.println(field);
        //输出：
        // public java.lang.String Demo.Person.name
        // public int Demo.Person.age
	}
	Field field=class1.getField("name");
	System.out.println(field);  //输出： public java.lang.String Demo.Person.name
}
```	


### 反射修改成员变量(Field)的值
- `public void set(Object obj, Object value)` :将指定对象变量上此 Field 对象表示的字段设置为指定的新值。obj指的是修改的是那个对象的这个成员变量值。

```java
public static void main(String[] args) throws Exception  {
	Class class1=Class.forName("Demo.Person");
	Field[] fields=class1.getFields();
		
	Field field=class1.getField("name");
	System.out.println(field);
		
	Object object=class1.newInstance();
	field.set(object, "张三");   //给name属性赋值“张三”
	System.out.println(object);   
}
```

### 反射获取空参数成员方法
- `public Method getMethod(String name, Class<?>... parameterTypes)` :返回一个 Method 对象，它反映此Class对象所表示的类或接口的指定公共成员方法。 
- `public Method[] getMethods()`:返回一个包含某些Method对象的数组，这些对象反映此Class对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共member方法。得到全部的成员方法(包括私有的，如果要使用私有成员方法，要先进行public void setAccessible(boolean flag) 设置。)
- `public Method getDeclaredMethod(String name, Class<?>... parameterTypes)` :返回一个 Method 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明方法。 
- `public Method[] getDeclaredMethods()`:返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。 
#### 使用Method方法对象
- `public Object invoke(Object obj, Object... args)` :对带有指定参数的指定对象调用由此 Method 对象表示的底层方法。 
	- `obj`指的是调这个方法的对象。
	- `args`指的是调用这个方法所要用到的参数列表。
	- 返回值`Object`就是方法的返回对象。如果方法没有返回值 ，返回的是null.

```java
public static void main(String[] args) throws Exception  {
	Class class1=Class.forName("Demo.Person");
	Object object=class1.newInstance();

	Method method=class1.getMethod("talk");
	method.invoke(object);  //调用名为"talk"的方法
}
```


### 反射获取有参数成员方法
- `public Method getMethod(String name, Class<?>... parameterTypes)` :返回一个 Method 对象，它反映此 Class 对象所表示的类或接口的指定公共成员方法。 
- `public Method[] getMethods()`:返回一个包含某些 Method 对象的数组，这些对象反映此 Class对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共 member 方法。得到全部的成员方法(包括私有的，如果要使用私有成员方法，要先进行public void setAccessible(boolean flag) 设置。)
- `public Method getDeclaredMethod(String name, Class<?>... parameterTypes)` :返回一个 Method 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明方法。 
- `public Method[] getDeclaredMethods()` :返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。 
#### 使用Method方法对象
- `public Object invoke(Object obj, Object... args)` :对带有指定参数的指定对象调用由此 Method 对象表示的底层方法。 
	- `obj`指的是调这个方法的对象。
	- `args`指的是调用这个方法所要用到的参数列表。
	- 返回值`Object`就是方法的返回对象。如果方法没有返回值 ，返回的是null.

```java
public static void main(String[] args) throws Exception  {
	Class class1=Class.forName("Demo.Person");
	Object object=class1.newInstance();
	//调用有参数的方法，传入参数类型的class对象
	Method method=class1.getMethod("talk",String.class);
	//调用对象的方法，并传入对应类型的参数值
	method.invoke(object,"人在说话！！");
}
```

### 反射泛型
```java
public static void main(String[] args) throws Exception  {
	ArrayList<String> list=new ArrayList<String>();
	Class class1=list.getClass();
		
	//获取ArrayList的add方法，给泛型集合添加数据
	Method method=class1.getMethod("add", Object.class);
	method.invoke(list, "zhangsan");
	method.invoke(list, 123);
	System.out.println(list);   //输出：[zhangsan, 123]
}
```