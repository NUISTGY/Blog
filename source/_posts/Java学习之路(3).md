---
title:  Java学习之路--常量
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-20

---
```java
/**
 * Java常量
 * @author 葛宇
 */
package 数据类型和运算符;

public class TestConstant {
	public static void main(String[] args) {
	  //age和name都是变量，数字18、20和字符串“GeYu”“GEYU”是字面常量
		int age = 18;
		age = 20;
		String name = "GeYu";
		name = "GEYU";
		
		final double PI = 3.14;
	             //PI++; 报错，final将PI定义为了符号常量
		
		/*
		 * 命名规范
		 * 常量：全部大写，可配合下划线：MAX_VALUE
		 * 类名：首字母大写，驼峰原则：HelloWorld、FatherClass
		 * 类成员变量、类方法：首字母小写，驼峰原则：monthSalary、getSalary()
		 */
	}
}

```
