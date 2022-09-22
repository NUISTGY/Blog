---
title:  Java学习之路--java标识符
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-20

---
```java
/**
 * Java标识符用法
 * @author 葛宇
 */
package 数据类型和运算符;

public class TestIdentifer {
	public static void main(String[] args) {
		int a123 = 1;
                             //int 123a = 2; 数字不能打头
		int $abc = 3;
		int _abc = 4; 
		int  年龄 = 18; //Java支持中文变量但不建议使用
	             //int public =5;   关键字不能作为变量去使用
	}
}

```
