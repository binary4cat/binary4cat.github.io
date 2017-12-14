---
title: Java基础：Set接口
date: 2017-06-16 23:39:46
categories: Java
tags: Java基础
permalink:
description: Java种的Set接口
photos: http://img.mp.sohu.com/upload/20170809/7f6678264b154d028f0e36e9159c8e9a.png
---
### Set接口
- Set接口的特点
    - Set接口是不能包含重复元素的集合。
    - Set集合取出元素的方式可以采用：迭代器、foreach。

### 实现类HashSet的存储和迭代
- HashSet (哈希表)无序集合,存储和取出的顺序不同,没有索引,不存储重复元素
- 内部使用的`map`存储的，添加操作在内部实现是当作map的key存储的，所以不会出现重复元素。
```java
public static void main(String[] args){
	Set<String> set=new HashSet<String>();
	set.add("a1");
	set.add("a2");
	set.add("a3");
	set.add("a3");   //相同数据会存储失败
	Iterator<String> iterator=set.iterator();
	while (iterator.hasNext()) {
		System.out.println(iterator.next());  //输出：a1 a2 a3
	}
    for (String str : set) {
		System.out.println(str);              //输出：a1 a2 a3
	}
}
```

  
### 哈希表的数据结构
    A:哈希表的数据结构:(参见图解)
       
        加载因子:表中填入的记录数/哈希表的长度
        例如:
        加载因子是0.75 代表:
          数组中的16个位置,其中存入16*0.75=12个元素

        如果在存入第十三个(>12)元素,导致存储链子过长,会降低哈希表的性能,那么此时会扩充哈希表(在哈希),底层会开辟一个长度为原长度2倍的数组,把老元素拷贝到新数组中,再把新元素添加数组中
          
        当存入元素数量>哈希表长度*加载因子,就要扩容,因此加载因子决定扩容时机

###12字符串对象的哈希值
      A:字符串对象的哈希值
      /*
       *  对象的哈希值,普通的十进制整数
       *  父类Object,方法 public int hashCode() 计算结果int整数
       */
      public class HashDemo {
        public static void main(String[] args) {
          Person p = new Person();
          int i = p.hashCode();
          System.out.println(i);
        
          String s1 = new String("abc");
          String s2 = new String("abc");
          System.out.println(s1.hashCode());
          System.out.println(s2.hashCode());
          
          /*System.out.println("重地".hashCode());
          System.out.println("通话".hashCode());*/
        }
      }
     
      //String类重写hashCode()方法
      //字符串都会存储在底层的value数组中{'a','b','c'}
      public int hashCode() {
              int h = hash;//hash初值为0
              if (h == 0 && value.length > 0) {
                  char val[] = value;

                  for (int i = 0; i < value.length; i++) {
                      h = 31 * h + val[i];
                  }
                  hash = h;
              }
              return h;
          }

        
###13哈希表的存储过程
   A:哈希表的存储过程
     public static void main(String[] args) {
        HashSet<String> set = new HashSet<String>();
        set.add(new String("abc"));
        set.add(new String("abc"));
        set.add(new String("bbc"));
        set.add(new String("bbc"));
        System.out.println(set); 
    }

  存取原理:
    每存入一个新的元素都要走以下三步:

    1.首先调用本类的hashCode()方法算出哈希值

    2.在容器中找是否与新元素哈希值相同的老元素,
      如果没有直接存入
      如果有转到第三步
    
    3.新元素会与该索引位置下的老元素利用equals方法一一对比
      一旦新元素.equals(老元素)返回true,停止对比,说明重复,不再存入
      如果与该索引位置下的老元素都通过equals方法对比返回false,说明没有重复,存入
 
=======================第四节课开始=============================================
###14哈希表的存储自定义对象
   A:哈希表的存储自定义对象
     /*
      *  HashSet集合的自身特点:
      *    底层数据结构,哈希表
      *    存储,取出都比较快
      *    线程不安全,运行速度快
      */
     public class HashSetDemo1 {
      public static void main(String[] args) {
        
        //将Person对象中的姓名,年龄,相同数据,看作同一个对象
        //判断对象是否重复,依赖对象自己的方法 hashCode,equals
        HashSet<Person> setPerson = new HashSet<Person>();
        setPerson.add(new Person("a",11));
        setPerson.add(new Person("b",10));
        setPerson.add(new Person("b",10));
        setPerson.add(new Person("c",25));
        setPerson.add(new Person("d",19));
        setPerson.add(new Person("e",17));//每个对象的地址值都不同,调用Obejct类的hashCode方法返回不同哈希值,直接存入
        System.out.println(setPerson);
      }
     }
    
    public class Person {
      private String name;
      private int age;
      
      public String getName() {
        return name;
      }
      public void setName(String name) {
        this.name = name;
      }
      public int getAge() {
        return age;
      }
      public void setAge(int age) {
        this.age = age;
      }
      public Person(String name, int age) {
        super();
        this.name = name;
        this.age = age;
      }
      public Person(){}
      
      public String toString(){
        return name+".."+age;
      }
      


     }


      

###15自定义对象重写hashCode和equals
	 A:自定义对象重写hashCode和equals
	  /*
          *  HashSet集合的自身特点:
          *    底层数据结构,哈希表
          *    存储,取出都比较快
          *    线程不安全,运行速度快
          */
         public class HashSetDemo1 {
          public static void main(String[] args) {
            
            //将Person对象中的姓名,年龄,相同数据,看作同一个对象
            //判断对象是否重复,依赖对象自己的方法 hashCode,equals
            HashSet<Person> setPerson = new HashSet<Person>();
            setPerson.add(new Person("a",11));
            setPerson.add(new Person("b",10));
            setPerson.add(new Person("b",10));
            setPerson.add(new Person("c",25));
            setPerson.add(new Person("d",19));
            setPerson.add(new Person("e",17));
            System.out.println(setPerson);
          }
         }
        
        public class Person {
          private String name;
          private int age;

          /*
           *  没有做重写父类,每次运行结果都是不同整数
           *  如果子类重写父类的方法,哈希值,自定义的
           *  存储到HashSet集合的依据
           *   
           *  尽可能让不同的属性值产生不同的哈希值,这样就不用再调用equals方法去比较属性
           *
           */
          public int hashCode(){
            return name.hashCode()+age*55;
          }
          //方法equals重写父类,保证和父类相同
          //public boolean equals(Object obj){}
          public boolean equals(Object obj){
            if(this == obj)
              return true;
            if(obj == null)
              return false;
            if(obj instanceof Person){
              Person p = (Person)obj;
              return name.equals(p.name) && age==p.age;
            }
            return false;
          }
          
          public String getName() {
            return name;
          }
          public void setName(String name) {
            this.name = name;
          }
          public int getAge() {
            return age;
          }
          public void setAge(int age) {
            this.age = age;
          }
          public Person(String name, int age) {
            super();
            this.name = name;
            this.age = age;
          }
          public Person(){}
          
          public String toString(){
            return name+".."+age;
          }
          


         }



###16LinkedHashSet集合
  A:LinkedHashSet集合
    /*
     *   LinkedHashSet 基于链表的哈希表实现
     *   继承自HashSet
     *   
     *   LinkedHashSet 自身特性,具有顺序,存储和取出的顺序相同的
     *   线程不安全的集合,运行速度块
     */
    public class LinkedHashSetDemo {
      
      public static void main(String[] args) {
        LinkedHashSet<Integer> link = new LinkedHashSet<Integer>();
        link.add(123);
        link.add(44);
        link.add(33);
        link.add(33);
        link.add(66);
        link.add(11);
        System.out.println(link);
      }
    }


###17ArrayList,HashSet判断对象是否重复的原因
  A:ArrayList,HashSet判断对象是否重复的原因
     a:ArrayList的contains方法原理:底层依赖于equals方法
       ArrayList的contains方法会使用调用方法时，
         传入的元素的equals方法依次与集合中的旧元素所比较，
         从而根据返回的布尔值判断是否有重复元素。
         此时，当ArrayList存放自定义类型时，由于自定义类型在未重写equals方法前，
         判断是否重复的依据是地址值，所以如果想根据内容判断是否为重复元素，需要重写元素的equals方法。
    
     b:HashSet的add()方法和contains方法()底层都依赖 hashCode()方法与equals方法()

      Set集合不能存放重复元素，其添加方法在添加时会判断是否有重复元素，有重复不添加，没重复则添加。
      HashSet集合由于是无序的，其判断唯一的依据是元素类型的hashCode与equals方法的返回结果。规则如下：
      先判断新元素与集合内已经有的旧元素的HashCode值
       如果不同，说明是不同元素，添加到集合。
       如果相同，再判断equals比较结果。返回true则相同元素；返回false则不同元素，添加到集合。
      所以，使用HashSet存储自定义类型，如果没有重写该类的hashCode与equals方法，则判断重复时，使用的是地址值，如果想通过内容比较元素是否相同，需要重写该元素类的hashcode与equals方法。


 
###18hashCode和equals方法的面试题 
 A:hashCode和equals的面试题
 /*
  *   两个对象  Person  p1 p2
  *   问题: 如果两个对象的哈希值相同 p1.hashCode()==p2.hashCode()
  *        两个对象的equals一定返回true吗  p1.equals(p2) 一定是true吗
  *        正确答案:不一定
  *        
  *        如果两个对象的equals方法返回true,p1.equals(p2)==true
  *        两个对象的哈希值一定相同吗
  *        正确答案: 一定
  */  
 在 Java 应用程序执行期间，
 1.如果根据 equals(Object) 方法，两个对象是相等的，那么对这两个对象中的每个对象调用 hashCode 方法都必须生成相同的整数结果。 
 2.如果根据 equals(java.lang.Object) 方法，两个对象不相等，那么对这两个对象中的任一对象上调用 hashCode 方法不 要求一定生成不同的整数结果。 
    
    两个对象不同(对象属性值不同) equals返回false=====>两个对象调用hashCode()方法哈希值相同
    
    两个对象调用hashCode()方法哈希值不同=====>equals返回true


    两个对象不同(对象属性值不同) equals返回false=====>两个对象调用hashCode()方法哈希值不同
    
    两个对象调用hashCode()方法哈希值相同=====>equals返回true
   
   所以说两个对象哈希值无论相同还是不同,equals都可能返回true
