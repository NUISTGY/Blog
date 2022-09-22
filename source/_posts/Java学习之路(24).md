---
title:  Java学习之路--参数传递机制
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-27

---
```java
/**
 * 测试JAVA中的参数传递机制
 * @author 葛宇
 */
package 面向对象;

public class TestValueTransport {
	
	int a;
	int b;
	
	TestValueTransport (int a,int b){
		this.a = a;
		this.b = b;
	}
	
	void testValueTrap(TestValueTransport x) {
		System.out.println(x);
	}
	
	public static void main(String[] args) {
		
		TestValueTransport obj = new TestValueTransport(1,2);
		TestValueTransport obj_ = obj;
		
		System.out.println(obj);
		System.out.println(obj_);
		obj_.testValueTrap(obj_);
		
		//输出三个对象变量引用的地址是相同的
		//要能区分对象变量和对象的区别：对象变量存储在栈中，对象存储在堆中
		//示例中的obj、obj_、x三者虽是不同的对象变量，但是始终指向同一个对象的地址
		//总结：Java中参数传值传递的是参数值的副本，但要明确对于对象变量来说，传递的副本就是对象的地址，所以指向还是同一个对象
	}	
}



```
