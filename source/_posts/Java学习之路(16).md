---
title:  Java学习之路--while循环
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-25

---
```java
/**
 * 测试while
 * @author 葛宇
 */
package 控制语句;

public class TestWhile {
	public static void main(String[] args) {
		/////////////////while/////////////////
		
		int i = 1;
		int sum = 0;
		//while的循环条件必须是布尔型，0或1无效
		while(i <= 100) {
			sum += i;
			i++;
		}
		System.out.println(sum);

		////////////////do-while////////////////
		
		i = 0;
		sum = 0;
		do {
			sum += i;
			i++;
		}while(i <= 100);
		System.out.println(sum);
	}
}


```
