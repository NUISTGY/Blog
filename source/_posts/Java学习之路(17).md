---
title:  Java学习之路--for循环
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-25

---
```java
/**
 *测试for循环
 *@author 葛宇
 */
package 控制语句;

public class TestFor {
	public static void main(String[] args) {
		int sum = 0;
		for(int i=1;i<=100;i++) {
			sum+=i;
		}
		System.out.println(sum);
		
		/*
		 * 1. 执行初始化语句：i=1
		 * 2. 执行判断：i<=100
		 * 3. 执行循环体：sum+=i
		 * 4. 执行步进：i++
		 * 5. 返回第2步
		 */
	}
}


```
