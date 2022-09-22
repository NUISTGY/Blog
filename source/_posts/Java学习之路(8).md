---
title:  Java学习之路--比较运算符
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-22

---
```java
/**
 * 测试比较运算符
 * @author 葛宇
 */
package 数据类型和运算符;

public class TestOperator_2 {
	public static void main(String[] args) {
	  //比较运算符仅根据变量的数值大小来进行比较
	  //可参与比较运算的有：byte/short/int/long,float/double,char
	  //比较运算返回值为布尔值：true，false
		int   a = 1;
		float b = 1;
		
		if(a == b) {
			System.out.println("==");
		}
		if(a != b) {
			System.out.println("!=");
		}
		if(a < b) {
			System.out.println("<");
		}
		if(a > b) {
			System.out.println(">");
		}
	}
}


```
