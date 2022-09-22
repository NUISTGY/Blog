---
title:  Java学习之路--方法
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-23

---
```java
/**
 * 测试Java方法
 * @author 葛宇
 */
package 控制语句;

public class TestMethod {
	public static void main(String[] args) {
		//通过对象调用普通方法
		TestMethod obj = new TestMethod();
		obj.Method_1();
		obj.Method_2(1, 2);
		int total = obj.Method_3(1, 2);
		System.out.println(total);		
	}
	
	void Method_1() {
		System.out.println("This is Method_1");
	}
	
	void Method_2(int a,int b) {
		int sum = a+b;
		System.out.println(sum);
	}
	
	int Method_3(int a,int b) {
		int sum = a+b;
		return sum;
		//return两个作用：结束方法的运行；返回值
	}
}

/*
 * 要点：
 * 实参的数目，数据类型，次序必须和对应方法的形参列表匹配
 * Java中的普通参数传递均为值传递 
 */



```
