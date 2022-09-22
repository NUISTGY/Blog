---
title:  Java学习之路--类型转换
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-22

---
```java
/**
 * 类型转换的误区
 * @author 葛宇
 */
package 数据类型和运算符;

public class TestTypeConvertError {
	public static void main(String[] args) {
		
		int a = 1000000000;			//10亿
		int b = 20;
		
		int ans1 = a*b;
		System.out.println(ans1);	//表达式范围越界
		
		long ans2 = a*b;
		System.out.println(ans2);	//表达式范围越界
		
		long ans3 = a*((long)b);
		System.out.print(ans3); 	//正确输出
	}
}

```
