---
title:  Java学习之路--switch多选择
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-25

---
```java
/**
 * 测试switch多选择
 * @author 葛宇
 */
package 控制语句;

public class TestSwitch {
	public static void main(String[] args) {
		int a = (int)(6*Math.random())+1;
		System.out.println("骰点："+a);
		
		switch(a) {
		case 1:
			System.out.println("1点");
			break;
		case 2:
			System.out.println("2点");
			break;
		case 3:
			System.out.println("3点");
			break;
		case 4:
			System.out.println("4点");
			break;
		case 5:
			System.out.println("5点");
			break;
		case 6:
			System.out.println("6点");
			break;
		default:
			break;
		
		//若省略break则会往下一条case穿透执行	
		}
	}
}


```
