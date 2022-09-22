---
title:  Java学习之路--循环语句中的continue
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-26

---
```java
/**
 * 测试循环中的continue
 * @author 葛宇
 */
package 控制语句;

public class TestContinue {
	public static void main(String[] args) {
		int count = 0;
		for(int i=1;i<=100;i++) {
			if(i%3==0) {
				//能被3整除则跳过这次循环
				continue;
			}
			//否则输出不能被3整除的数
			System.out.print(i+"  ");
			count++;
			if(count%5==0) {
				System.out.println();
			}
		}
	}
}

```
