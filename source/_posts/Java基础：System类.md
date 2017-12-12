---
title: Java基础：System类
date: 2017-05-28 21:48:41
categories: Java
tags: Java基础
permalink:
description: System类
photos: http://ww1.sinaimg.cn/large/c55a7aeely1fmbwbz5s71j20et08cjra.jpg
---
###08System类方法currentTimeMillis 
   *A:System类方法currentTimeMillis():用于计算程序的执行时间
        /*
      	 *  获取系统当前毫秒值
      	 *  static long currentTimeMillis()
      	 *  对程序执行时间测试
      	 */
      	public static void function(){
      		long start = System.currentTimeMillis();//当前时间x-1970年1月1日零时零分零秒
      		for(int i = 0 ; i < 10000; i++){
      			System.out.println(i);
      		}
      		long end = System.currentTimeMillis();//当前时间y-1970年1月1日零时零分零秒
      		System.out.println(end - start);//当前时间y-当前时间x 
      	}
 
###09System类方法exit 
     *A:System类方法exit()方法
		     /*
		 	 *  退出虚拟机,所有程序全停止
		 	 *  static void exit(0)
		 	 */
		 	public static void function_1(){
		 		while(true){
		 			System.out.println("hello");
		 			System.exit(0);//该方法会在以后的finally代码块中使用(讲到再说)
		 		}
		 	}
###10System类方法gc 
   A:System类方法gc
        public class Person {
        	public void finalize(){
        		System.out.println("垃圾收取了");
        	}
        }

        /*
     	 *  JVM在内存中,收取对象的垃圾
     	 *  当没有更多引用指向该对象时,会自动调用垃圾回收机制回收堆中的对象
     	 *  同时调用回收对象所属类的finalize方法()
     	 *  static void gc()
     	 */
     	public static void function_2(){
     		new Person();
     		new Person();
     		new Person();
     		new Person();
     		new Person();
     		new Person();
     		new Person();
     		new Person();
     		System.gc();
     	}

###11System类方法getProperties 
  A:System类方法getProperties(了解)
     /*
      *  获取当前操作系统的属性:例如操作系统名称,
      *  static Properties getProperties() 
      */
     public static void function_3(){
     	System.out.println( System.getProperties() );
     }
      
###12System类方法arraycopy
	 A:System类方法arraycopy：
	  /*
	   * System类方法,复制数组
	   * arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
	   * Object src, 要复制的源数组
	   * int srcPos, 数组源的起始索引
	   * Object dest,复制后的目标数组
	   * int destPos,目标数组起始索引 
	   * int length, 复制几个
	   */
	  public static void function_4(){
	  	int[] src = {11,22,33,44,55,66};
	  	int[] desc = {77,88,99,0};
	  	
	  	System.arraycopy(src, 1, desc, 1, 2);//将src数组的1位置开始(包含1位置)的两个元素,拷贝到desc的1,2位置上
	  	for(int i = 0 ;  i < desc.length ; i++){
	  		System.out.println(desc[i]);
	  	}
	  }

