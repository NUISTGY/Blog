---
title:  Java学习之路--this的使用
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-27

---
```java
/**
 * 测试this的使用
 * @author 葛宇
 */
package 面向对象;

public class TestThis {
	int a,b,c;
	
	//constructor 1
	TestThis(int a,int b){
		this.a = a;
		this.b = b;
		//发生变量二义性时，使用this可以指代创建出来的对象
	}	
	//constructor 2
	TestThis(int a,int b,int c){
	  //TestThis(a,b);会报错，想在类方法中重用构造器得用this，如下
		this(a,b);//这里等价于调用了constructor 1，且必须位于构造器的第一句
		this.c = c;
	}
	
	void func1() {
		System.out.println("this is func1 !");
	}
	
	void func2() {
		this.func1();
		System.out.println("this is func2 !");
		//this可以用来指代该类的某个对象自己
	}
	
	public static void main(String[] args) {
		TestThis obj = new TestThis(1,2,3);
		obj.func2();
	}
}

//注意，this的使用基本上都是离不开对象的，所以要知道this是不能用于静态的方法的。



```
