---
title:  Java学习之路--final关键字
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-28

---
```java
/**
 * 测试final关键字
 * @author 葛宇
 */
package 面向对象核心;

public class TestFinal {
	public static void main(String[] args) {
		A a = new A();
		B b = new B();
		
	  //a.a = 2;	//报错，final修饰变量则不可变
		b.b = 2;	//无final修饰，可自行赋值
	}
}

final class A{
	
	final int a = 1;
}

class B /*extends A*  报错，final修饰的类无法被继承*/{
	
	int b;
	
	final public void func(int x) {
		//...
	}
	
	public void func(double x) {
		//final修饰的方法可以在本类中重载
	}
}

class C extends B{
	
   /*	public void func(int x) {
	*		//...
	*	}
	*final修饰的方法无法被子类重写
	*/
}

```
