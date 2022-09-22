---
title:  Java学习之路--If选择语句
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-25

---
```java
/**
 * If选择语句
 * @author 葛宇
 */
package 控制语句;

public class TestIf {
	public static void main(String[] args) {
		double x = 6*Math.random();
		int age = (int)(80*Math.random());		
		System.out.println(x);
		System.out.println(age);
		
		////////////if///////////
		
		if(x <= 2) {
			System.out.println("Small");
		}
		if(x >= 2) {
			System.out.println("Large");
		}
		
		//////////if-else/////////
		
		if(x <= 2) {
			System.out.println("Small");
		}else {
			System.out.println("Large");
		}
		
		///////Multi-if-else///////
		
		if(age <= 15) {
			System.out.println("儿童");
		}else if(age <= 25) {
			System.out.println("青年");
		}else if(age <= 45) {
			System.out.println("中年");
		}else if(age <= 80) {
			System.out.println("老年");
		}
	}
}


```
