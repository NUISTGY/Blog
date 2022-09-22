---
title:  Java学习之路--类和对象
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-27

---
```java
/**
   *   测试类和对象
 * @author 葛宇
 */
package 面向对象;

//一个文件可以有多个类，但是public修饰的类只能有一个
public class TestFirstClass {
	//开始就以学生为对象做示例
	
	//属性
	int id;
	int age;
	String name;
	Subjects subject;
	
	//方法
	void study() {
		System.out.println("学习"+subject.name);
	}
	
	void show() {
		System.out.println("ID:"+this.id);
		System.out.println("AGE:"+this.age);
		System.out.println("NAME:"+this.name);
	}
	
	TestFirstClass(){
		//构造方法，用于创建类对象，方法名与类名保持一致，无返回值，无参构造方法可由系统自动生成	
	}
	
	
	//main（）方法程序执行的入口，必不可少
	public static void main(String[] args) {
		TestFirstClass stu = new TestFirstClass();		//调用构造方法在堆区创建对象，返回对象在堆区的地址
		Subjects sub = new Subjects();					//调用构造方法在堆区创建对象，返回对象在堆区的地址
		
		//对象属性的赋值
		sub.name = "JAVA课设";
		//对象属性的赋值
		stu.id = 12345;
		stu.age = 20;
		stu.name = "葛宇";
		stu.subject = sub;	
		
		stu.study();		//调用类方法
		stu.show();			//调用类方法
		
		System.out.println(sub);			//打印对象地址
		System.out.println(stu.subject);	//打印对象地址
	  //观察发现输出两个对象地址相同，原因是sub放于堆中，44行的赋值实际上是把sub的堆地址赋值给了stu.subject
	  //分析可知：sub和stu.subject都位于栈区，都指向堆区的同一个对象地址
	}
}

//学科科目类
class Subjects{
	String name;
}

/* JAVA内存分析
 * JAVA中主要把内存分为三部分：栈、堆、方法区（其中堆区包含方法区）
 * 
 * 	栈：JVM会为每个线程创建一个栈且栈属于线程私有，无法与其他线程共享；
 * 	        栈的存储特性是“先进后出”；
 * 	        栈是自动分配，内存空间连续，速度快；
 * 	        程序中的每个方法执行时都会在栈中创建一个栈帧，用于存放该方法运行时数据、局部变量、实参、返回值等信息；
 * 
 * 	堆：堆区存放创建好的对象和数组（数组也是对象）；
 * 	   JVM只有一个堆区，且被所有线程共享；
 * 	         堆区是一个不连续的内存空间，分配灵活，速度慢；
 * 
 * 	方法区（静态区）：方法区也是堆区的一部分，只有一个，被所有线程共享；
 * 				方法区中存放程序中不变或唯一的内容：类代码、静态变量、常量、字符串等；
 */

```
