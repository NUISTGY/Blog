---
title:  Java学习之路--static关键字
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-28

---
```java
/**
 * 测试static关键字
 * @author 葛宇
 */
package 面向对象;

public class TestStatic {
	
	int a;
	int b;
	
	static int x;	//属于类而不属于对象
	static int y;	//属于类而不属于对象
	
	void func1() {
		//普通成员方法可以调用类的普通成员变量和static变量和static方法
		System.out.println(a);
		System.out.println(b);
		System.out.println(x);
		System.out.println(y);
		System.out.println("普通成员方法func1");
		func2();
	}
	
	static void func2() {
		//属于类而不属于对象
		//不可在static方法调用this.a或this.b或this.func1()
		//static方法的生命周期与类相同，早于对象的诞生，而普通成员变量是属于对象的，所以static方法无法调用普通成员变量
		System.out.println("static成员方法func2");
	}
	
	public static void main(String[] args) {
		
		TestStatic obj = new TestStatic();
		x = 1;									//可以不通过对象直接调用static成员变量
		System.out.println(x);					//可以不通过对象直接调用static成员变量
		System.out.println(TestStatic.x);		//可以通过对象调用static成员变量
		
		func2();								//可以不通过对象直接调用static成员方法
		TestStatic.func2();						//可以通过对象调用static成员变量
		
		TestStatic obj1 = new TestStatic();
		TestStatic.x ++;
		System.out.println(TestStatic.x);
		//静态变量的改变相当于是对类模板的改变，会对其他对象产生影响，
	}
}


```
