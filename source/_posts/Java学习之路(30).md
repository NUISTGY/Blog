---
title:  Java学习之路--Object类并重写toString
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-28

---
```java
/**
 * 测试Object类并重写toString
 * @author 葛宇
 */
package 面向对象核心;

public class TestObject {
	public static void main(String[] args) {
		TestObject obj = new TestObject();
		System.out.println(obj.toString());
		
		People ple = new People("Kevin",18);
		System.out.println(ple);
	}
	
	@Override
	public String toString() {
		return "测试Overtide Object类的toString方法";
	}	
}

class People{
	int age;
	String name;
	
	@Override
	public String toString() {
		return name+" 年龄："+age;
	}	
	
	public People(String name,int age) {
		this.name = name;
		this.age = age;
	}
}
```
