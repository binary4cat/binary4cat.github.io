---
title: Java基础：数值类型操作
date: 2017-05-30 23:51:32
categories: Java
tags: Java基础
permalink:
description: Java基础：数值类型操作
photos: http://ww1.sinaimg.cn/large/c55a7aeely1fmbwbz5s71j20et08cjra.jpg
---
###13Math类的方法_1
   A:Math类中的方法
   /*
   	 * static double sqrt(double d)
   	 * 返回参数的平方根
   	 */
   	public static void function_4(){
   		double d = Math.sqrt(-2);
   		System.out.println(d);
   	}
   	
   	/*0
   	 * static double pow(double a, double b)
   	 * a的b次方
   	 */
   	public static void function_3(){
   		double d = Math.pow(2, 3);
   		System.out.println(d);
   	}
   	
   	/*
   	 * static double floor(double d)
   	 * 返回小于或者等于参数d的最大整数
   	 */
   	public static void function_2(){
   		double d = Math.floor(1.5);
   		System.out.println(d);
   	}
   	
   	/*
   	 *  static double ceil(double d)
   	 *  返回大于或者等于参数d的最小整数
   	 */
   	public static void function_1(){
   		double d = Math.ceil(5.1);
   		System.out.println(d);
   	}
   	
   	/*
   	 *  static int abs(int i)
   	 *  获取参数的绝对值
   	 */
   	 public static void function(){
   		int i = Math.abs(0);
   		System.out.println(i);
   	 }

###14Math类的方法_2
 A:Math类的方法_2
  /*
   *  static double round(doubl d)
   *  获取参数的四舍五入,取整数
   */
  public static void function_6(){
  	double d = Math.round(5.4195);
  	System.out.println(d);
  }
  
  /*
   *  static double random() 返回随机数 0.0-1.0之间
   *  来源,也是Random类
   */
  public static void function_5(){
  	for(int i = 0 ; i < 10 ;i++){
  		double d = Math.random();
  		System.out.println(d);
  	}
  }
###17BigInteger类概述和构造方法   
 A:BigInteger类概述和构造方法
   public static void main(String[] args) {
   		function();
   	}
    /*
   	 * BigInteger类的构造方法
   	 * 传递字符串,要求数字格式,没有长度限制
   	 */
   	public static void function(){
   		BigInteger b = new BigInteger("8465846668464684562385634168451684568645684564564");
   		System.out.println(b);
   		BigInteger b1 = new BigInteger("5861694569514568465846668464684562385634168451684568645684564564");
   		System.out.println(b1);
   	}

###18BigInteger类四则运算  
 A:BigInteger类四则运算
    public static void main(String[] args) {
   		function_1();
   	}
	/*
	 * BigInteger对象的四则运算
	 * 调用方法计算,计算结果也只能是BigInteger对象
	 */
	 public static void function_1(){
		 BigInteger b1 = new BigInteger("5665464516451051581613661405146");
		 BigInteger b2 = new BigInteger("965855861461465516451051581613661405146");
		 
		 //计算 b1+b2对象的和,调用方法 add
		 BigInteger bigAdd = b1.add(b2);//965855867126930032902103163227322810292
		 System.out.println(bigAdd);
		 
		 //计算b1-b2对象的差,调用方法subtract
		 BigInteger bigSub = b1.subtract(b2);
		 System.out.println(bigSub);
		 
		 //计算b1*b2对象的乘积,调用方法multiply
		 BigInteger bigMul = b1.multiply(b2);
		 System.out.println(bigMul);
		 
		 //计算b2/b1对象商,调用方法divied
		 BigInteger bigDiv = b2.divide(b1);
		 System.out.println(bigDiv);
	 }

###19员工案例的子类的编写
 A:BigDecimal类概述 
    
    /*
     * 计算结果,未知
     * 原因: 计算机二进制中,表示浮点数不精确造成
     * 超级大型的浮点数据,提供高精度的浮点运算, BigDecimal
    System.out.println(0.09 + 0.01);//0.09999999999999999
    System.out.println(1.0 - 0.32);//0.6799999999999999
    System.out.println(1.015 * 100);//101.49999999999999
    System.out.println(1.301 / 100);//0.013009999999999999 
    */

###20BigDecimal类实现加法减法乘法  
 A:BigDecimal类实现加法减法乘法
  /*
  	 *  BigDecimal实现三则运算
  	 *  + - *
  	 */
  	public static void function(){
  		BigDecimal b1 =  new BigDecimal("0.09");
  		BigDecimal b2 =  new BigDecimal("0.01");
  		//计算b1+b2的和,调用方法add
  		BigDecimal bigAdd = b1.add(b2);
  		System.out.println(bigAdd);
  		
  		BigDecimal b3 = new BigDecimal("1");
  		BigDecimal b4 = new BigDecimal("0.32");
  		//计算b3-b2的差,调用方法subtract
  		BigDecimal bigSub = b3.subtract(b4);
  		System.out.println(bigSub);
  		
  		BigDecimal b5 = new BigDecimal("1.015");
  		BigDecimal b6 = new BigDecimal("100");
  		//计算b5*b6的成绩,调用方法 multiply
  		BigDecimal bigMul = b5.multiply(b6);
  		System.out.println(bigMul);
  	}

###21BigDecimal类实现除法
	 A:BigDecimal类实现除法
	 /*
	  * BigDecimal实现除法运算
	  * divide(BigDecimal divisor, int scale, int roundingMode) 
	  * int scale : 保留几位小数
	  * int roundingMode : 保留模式
	  * 保留模式 阅读API文档
	  *   static int ROUND_UP  向上+1
	  *   static int ROUND_DOWN 直接舍去
	  *   static int ROUND_HALF_UP  >= 0.5 向上+1
	  *   static int ROUND_HALF_DOWN   > 0.5 向上+1 ,否则直接舍去
	  */
	 public static void function_1(){
	 	BigDecimal b1 = new BigDecimal("1.0301");
	 	BigDecimal b2 = new BigDecimal("100");
	 	//计算b1/b2的商,调用方法divied
	 	BigDecimal bigDiv = b1.divide(b2,2,BigDecimal.ROUND_HALF_UP);//0.01301
	 	System.out.println(bigDiv);
	 }
