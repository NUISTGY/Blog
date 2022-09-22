---
title:  Java学习之路--从键盘读入数据
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-23

---
```java
/**
 * 测试从键盘读入数据
 * @author 葛宇
 */
package 数据类型和运算符;
import java.util.Scanner;

public class TestScanner {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);	//创建IO流对象
		System.out.println("请输入姓名：");
		String name = scanner.nextLine();
		System.out.println("请输入性别：");
		String sex = scanner.nextLine();
		System.out.println("请输入ID：");
		int age = scanner.nextInt();
		
		System.out.println(name);
		System.out.println(sex);
		System.out.println(age);
	}
}


```
