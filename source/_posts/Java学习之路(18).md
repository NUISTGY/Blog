---
title:  Java学习之路--循环语句中的break
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-26

---
```java
/**
 * 测试循环语句中的break
 * @author 葛宇
 */
package 控制语句;

public class TestBreak {
	public static void main(String[] args) {
		int total = 0;
		while(true) {
			int i = (int)Math.round(100*Math.random());
			System.out.println(i);
			if(i==50) {
				break;
			}
			total++;
		}
		System.out.println("循环次数为："+total);
	}
}
//输出循环次数为30

```
