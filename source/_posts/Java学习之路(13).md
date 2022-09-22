---
title:  Java学习之路--方法的重载
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-23

---
```java
/**
 * 测试方法的重载
 * @author 葛宇
 */
package 控制语句;

public class TestOverload {
	public static void main(String[] args) {
		
	}
	
	/*求和的方法，static修饰，调用时无需创建对象*/
	public static int add(int n1,int n2) {
		int sum = n1+n2;
		return sum;
	}
	//方法名相同，参数个数不同，构成重载
	public static int add(int n1,int n2,int n3) {
		int sum = n1+n2+n3;
		return sum;
	}
	//方法名相同，参数类型不同，构成重载
	public static double add(double n1,int n2) {
		double sum = n1+n2;
		return sum;
	}
	//方法名相同，参数顺序不同，构成重载
	public static double add(int n1,double n2) {
		double sum = n1+n2;
		return sum;
	}
	
	/**方法名相同，返回值类型不同，无法构成重载，报错！！！
	 	public static double add(int n1,int n2) {
		double sum = n1+n2;
		return sum;
	}
	 */
	
	/**方法名相同，形参名不同，无法构成重载，报错！！！
 		public static double add(int n2,int n1) {
		double sum = n1+n2;
		return sum;
	}
	 */
}


```
