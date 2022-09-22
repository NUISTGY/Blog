---
title:  Java学习之路--继承
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-28

---
```java
/**
 * 测试继承
 * @author 葛宇
 */
package 面向对象核心;

public class TestExtends {
	public static void main(String[] args) {
		Student stu = new Student();
		Student stu_ = new Student("Max",170,"C++");
	  //stu拥有Person类所以属性和方法
		stu.name = "Jack";
		stu.height = 180;
		stu.major = "JAVA";
		stu.breath();
		
		System.out.println(stu instanceof Student);
		System.out.println(stu instanceof Person);
		System.out.println(stu instanceof Object);
	}
}

class Person{
	
	int height;
	String name;
	
	public void breath() {
		System.out.println("呼吸");
	}
}

class Student extends Person{
	
	//int height;
	//String name;
	String major;
	
	public Student() {
		
	}
	
	public Student(String name,int height,String major) {
		this.name = name;
		this.height = height;
		this.major = major;
	}
	
	public void study() {
		System.out.println("学习");
	}
	
/*	public void breath() {
*		System.out.println("呼吸");
*	}
*/	
}

/*
*父类也叫超类或基类
*没有多继承，只有单继承
*接口技术有多继承
*java中只有共有继承
*子类可以得到父类的全部属性和方法（构造方法除外），但不见得都可以使用
*如果没有使用extends继承则默认是继承了java.lang.Object
*/

```
