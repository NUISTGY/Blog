---
title:  Java学习之路--算数运算符
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-22

---
```java
/**
 * 测试算数运算符
 * @author 葛宇
 */
package 数据类型和运算符;

public class TsetOperator_1 {
	public static void main(String[] args) {
		/*
		 * 整数运算：
		 * 如果整数操作数里有long，则结果也为long
		 * 没有long时，结果为int。即使操作数全为short、byte，结果也是int
		 */
		byte a = 1;
		int  b = 2;
		int  ans1 = a+b;
	  //byte ans2 = a+b;  报错
		
		long c = 3;
		long ans3 = b+c;
	  //int  ans4 = b+c;  报错
		
		/*
		 * 浮点运算：
		 * 如果操作数涉及浮点数,
		 * 如果两个浮点数有一个为double，则结果为double
		 * 只有两个浮点数数都为float，结果才为float
		 */
		float d = 1;
		float e = 2;
		double f = 3;
		float ans5 = d+e;
        double ans6 = d+e;
        double ans7 = e+f;
      //float ans8 = e+f;  报错
        
        /*
		 * 整数与浮点运算：
		 * 只要操作数涉及浮点数，则向浮点数兼容
		 */
        
        /*
                           * 取模运算
                           *结果（余数）的符号与左操作数相同 
         */
        System.out.println(9%5);
        System.out.println(-9%5);
        System.out.println(9%-5);
		
	}
}

```
