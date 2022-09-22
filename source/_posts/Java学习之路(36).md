---
title:  Java学习之路--数组用法(2)
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-31

---
```java
/**
 * 测试数组
 * @author 葛宇
 * 测试三种初始化方法和foreach遍历
 */
package com.github.nuistgy.testarray;

public class Test02 {
	public static void main(String[] args) {
	  //静态初始化
		int a[] = {1,2,3};
		Student stu[] = {new Student(1001,"Mark"),
						 new Student(1002,"Jhon"),
						 new Student(1003,"Jack")
						 };
	  //默认初始化	
		int b[] = new int[3];				//默认是0
		boolean bool[] = new boolean[3];	//默认是false
		String s[] = new String[3];			//默认是null
		Student stu_[] = new Student[3]; 	//默认是null
		
	  //动态初始化
		int c[] = new int[3];
		c[0] = 1;
		c[1] = 2;
		c[2] = 3;
		
	  //foreach遍历.注意：foreach不用于写操作，只用于读操作
		for(int m :c) {
			System.out.println(m);
		}
	}
}

class Student{
	private int id;
	private String  name;
	
	Student(int id,String name){
		this.id = id;
		this.name = name;
	}
}
```
